---
title: "Kurz: Azure Active Directory integrace s cloudem SAP zákazníka. | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP cloudu pro zákazníka."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: e4d945525a45704f34e1d9e742220928a516f341
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a><span data-ttu-id="e4055-103">Kurz: Azure Active Directory integrace s cloudem SAP zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e4055-103">Tutorial: Azure Active Directory integration with SAP Cloud for Customer</span></span>

<span data-ttu-id="e4055-104">V tomto kurzu zjistěte, jak integrovat SAP cloudu pro zákazníka s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e4055-104">In this tutorial, you learn how to integrate SAP Cloud for Customer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e4055-105">Integrace SAP cloudu pro zákazníka s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e4055-105">Integrating SAP Cloud for Customer with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e4055-106">Můžete řídit ve službě Azure AD, který má přístup do cloudu SAP zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e4055-106">You can control in Azure AD who has access to SAP Cloud for Customer</span></span>
- <span data-ttu-id="e4055-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k SAP cloudu pro zákazníka (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4055-107">You can enable your users to automatically get signed-on to SAP Cloud for Customer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e4055-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e4055-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e4055-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e4055-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4055-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e4055-110">Prerequisites</span></span>

<span data-ttu-id="e4055-111">Ke konfiguraci integrace služby Azure AD s cloudem SAP pro zákazníka, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e4055-111">To configure Azure AD integration with SAP Cloud for Customer, you need the following items:</span></span>

- <span data-ttu-id="e4055-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4055-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e4055-113">Cloud SAP pro zákazníka jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e4055-113">A SAP Cloud for Customer single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e4055-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e4055-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e4055-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e4055-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e4055-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e4055-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e4055-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4055-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e4055-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e4055-118">Scenario description</span></span>
<span data-ttu-id="e4055-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e4055-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e4055-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e4055-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e4055-121">Přidání SAP cloudu pro zákazníka z Galerie</span><span class="sxs-lookup"><span data-stu-id="e4055-121">Adding SAP Cloud for Customer from the gallery</span></span>
2. <span data-ttu-id="e4055-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4055-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a><span data-ttu-id="e4055-123">Přidání SAP cloudu pro zákazníka z Galerie</span><span class="sxs-lookup"><span data-stu-id="e4055-123">Adding SAP Cloud for Customer from the gallery</span></span>
<span data-ttu-id="e4055-124">Při konfiguraci integrace SAP cloudu pro zákazníka do služby Azure AD, potřebujete přidat SAP cloudu pro zákazníka z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e4055-124">To configure the integration of SAP Cloud for Customer into Azure AD, you need to add SAP Cloud for Customer from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e4055-125">**Pokud chcete přidat SAP cloudu pro zákazníka z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="e4055-125">**To add SAP Cloud for Customer from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e4055-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e4055-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e4055-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e4055-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e4055-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e4055-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e4055-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4055-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e4055-133">Do vyhledávacího pole zadejte **SAP cloudu pro zákazníka**.</span><span class="sxs-lookup"><span data-stu-id="e4055-133">In the search box, type **SAP Cloud for Customer**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. <span data-ttu-id="e4055-135">Na panelu výsledků vyberte **SAP cloudu pro zákazníka**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e4055-135">In the results panel, select **SAP Cloud for Customer**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e4055-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4055-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e4055-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s cloudem SAP pro zákazníka na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e4055-138">In this section, you configure and test Azure AD single sign-on with SAP Cloud for Customer based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e4055-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v cloudu SAP pro zákazníka je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e4055-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAP Cloud for Customer is to a user in Azure AD.</span></span> <span data-ttu-id="e4055-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v cloudu SAP pro zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e4055-140">In other words, a link relationship between an Azure AD user and the related user in SAP Cloud for Customer needs to be established.</span></span>

<span data-ttu-id="e4055-141">V cloudu SAP pro zákazníka, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e4055-141">In SAP Cloud for Customer, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e4055-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s cloudem SAP pro zákazníka, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e4055-142">To configure and test Azure AD single sign-on with SAP Cloud for Customer, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e4055-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e4055-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e4055-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4055-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e4055-145">**[Vytvoření cloudu SAP pro zákazníka testovacího uživatele](#creating-a-sap-cloud-for-customer-test-user)**  – Pokud chcete mít protějšek Britta Simon v cloudu SAP zákazníkovi, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e4055-145">**[Creating a SAP Cloud for Customer test user](#creating-a-sap-cloud-for-customer-test-user)** - to have a counterpart of Britta Simon in SAP Cloud for Customer that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e4055-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e4055-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e4055-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e4055-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e4055-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4055-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e4055-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování ve vašem cloudu SAP pro aplikace pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="e4055-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAP Cloud for Customer application.</span></span>

<span data-ttu-id="e4055-150">**Ke konfiguraci Azure AD jednotné přihlašování s cloudem SAP pro zákazníka, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4055-150">**To configure Azure AD single sign-on with SAP Cloud for Customer, perform the following steps:**</span></span>

1. <span data-ttu-id="e4055-151">Na portálu Azure na **SAP cloudu pro zákazníka** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e4055-151">In the Azure portal, on the **SAP Cloud for Customer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e4055-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e4055-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. <span data-ttu-id="e4055-155">Na **SAP cloudu zákazníka domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4055-155">On the **SAP Cloud for Customer Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    <span data-ttu-id="e4055-157">a.</span><span class="sxs-lookup"><span data-stu-id="e4055-157">a.</span></span> <span data-ttu-id="e4055-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="e4055-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    <span data-ttu-id="e4055-159">b.</span><span class="sxs-lookup"><span data-stu-id="e4055-159">b.</span></span> <span data-ttu-id="e4055-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="e4055-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e4055-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e4055-161">These values are not real.</span></span> <span data-ttu-id="e4055-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e4055-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e4055-163">Obraťte se na [SAP Cloud pro tým podpory zákazníků klienta](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e4055-163">Contact [SAP Cloud for Customer Client support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) to get these values.</span></span> 

4. <span data-ttu-id="e4055-164">Na **uživatelské atributy** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4055-164">On the **User Attributes** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    <span data-ttu-id="e4055-166">a.</span><span class="sxs-lookup"><span data-stu-id="e4055-166">a.</span></span> <span data-ttu-id="e4055-167">V **uživatelský identifikátor** seznamu, vyberte **ExtractMailPrefix()** funkce.</span><span class="sxs-lookup"><span data-stu-id="e4055-167">In **User Identifier** list, select the **ExtractMailPrefix()** function.</span></span>

    <span data-ttu-id="e4055-168">b.</span><span class="sxs-lookup"><span data-stu-id="e4055-168">b.</span></span> <span data-ttu-id="e4055-169">Z **e-mailu** vyberte atribut uživatele, kterou chcete použít týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="e4055-169">From the **Mail** list, select the user attribute you want to use for your implementation.</span></span>
    <span data-ttu-id="e4055-170">Například pokud chcete použít EmployeeID jako jedinečný identifikátor uživatele a hodnota atributu jsou uloženy v ExtensionAttribute2, pak vyberte user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="e4055-170">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select user.extensionattribute2.</span></span>  

5. <span data-ttu-id="e4055-171">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e4055-171">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. <span data-ttu-id="e4055-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e4055-173">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="e4055-175">Na **SAP cloudu pro konfiguraci zákazníka** klikněte na tlačítko **konfigurace cloudu SAP pro zákazníka** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e4055-175">On the **SAP Cloud for Customer Configuration** section, click **Configure SAP Cloud for Customer** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e4055-176">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e4055-176">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. <span data-ttu-id="e4055-178">Potřebujete nakonfigurovat jednotné přihlašování, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4055-178">To get SSO configured, perform the following steps:</span></span>
   
    <span data-ttu-id="e4055-179">a.</span><span class="sxs-lookup"><span data-stu-id="e4055-179">a.</span></span> <span data-ttu-id="e4055-180">Přihlášení do cloudu SAP pro zákaznický portál s právy správce.</span><span class="sxs-lookup"><span data-stu-id="e4055-180">Login into SAP Cloud for Customer portal with administrator rights.</span></span>
   
    <span data-ttu-id="e4055-181">b.</span><span class="sxs-lookup"><span data-stu-id="e4055-181">b.</span></span> <span data-ttu-id="e4055-182">Přejděte na **aplikace a běžné úlohy správy uživatele** a klikněte na tlačítko **zprostředkovatele Identity** kartě.</span><span class="sxs-lookup"><span data-stu-id="e4055-182">Navigate to the **Application and User Management Common Task** and click the **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="e4055-183">c.</span><span class="sxs-lookup"><span data-stu-id="e4055-183">c.</span></span> <span data-ttu-id="e4055-184">Klikněte na tlačítko **nového poskytovatele Identity** a vyberte soubor XML metadat, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e4055-184">Click **New Identity Provider** and select the metadata XML file you have downloaded from the Azure portal.</span></span> <span data-ttu-id="e4055-185">Importováním metadata systém automaticky nahrává dodejka certifikát a certifikát pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="e4055-185">By importing the metadata, the system automatically uploads the required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    <span data-ttu-id="e4055-187">d.</span><span class="sxs-lookup"><span data-stu-id="e4055-187">d.</span></span> <span data-ttu-id="e4055-188">Azure Active Directory vyžaduje element adresa URL služby Assertion příjemce v žádosti o SAML, vyberte **zahrnují Assertion příjemce adresa URL služby** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="e4055-188">Azure Active Directory requires the element Assertion Consumer Service URL in the SAML request, so select the **Include Assertion Consumer Service URL** checkbox.</span></span>
   
    <span data-ttu-id="e4055-189">e.</span><span class="sxs-lookup"><span data-stu-id="e4055-189">e.</span></span> <span data-ttu-id="e4055-190">Klikněte na tlačítko **aktivovat jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e4055-190">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="e4055-191">f.</span><span class="sxs-lookup"><span data-stu-id="e4055-191">f.</span></span> <span data-ttu-id="e4055-192">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="e4055-192">Save your changes.</span></span>
   
    <span data-ttu-id="e4055-193">g.</span><span class="sxs-lookup"><span data-stu-id="e4055-193">g.</span></span> <span data-ttu-id="e4055-194">Klikněte **Moje systému** kartě.</span><span class="sxs-lookup"><span data-stu-id="e4055-194">Click the **My System** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    <span data-ttu-id="e4055-196">h.</span><span class="sxs-lookup"><span data-stu-id="e4055-196">h.</span></span> <span data-ttu-id="e4055-197">V **Azure AD adresa URL přihlašování** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e4055-197">In **Azure AD Sign On URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    <span data-ttu-id="e4055-199">i.</span><span class="sxs-lookup"><span data-stu-id="e4055-199">i.</span></span> <span data-ttu-id="e4055-200">Zadejte, zda zaměstnanec ručně vybrat mezi přihlásit pomocí ID uživatele a heslo nebo jednotného přihlašování k výběrem **výběr zprostředkovatele Identity ruční**.</span><span class="sxs-lookup"><span data-stu-id="e4055-200">Specify whether the employee can manually choose between logging on with user ID and password or SSO by selecting the **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="e4055-201">j.</span><span class="sxs-lookup"><span data-stu-id="e4055-201">j.</span></span> <span data-ttu-id="e4055-202">V **jednotného přihlašování k adrese URL** části, zadejte adresu URL, která má být zaměstnancům používána k přihlášení k systému.</span><span class="sxs-lookup"><span data-stu-id="e4055-202">In the **SSO URL** section, specify the URL that should be used by your employees to sign on to the system.</span></span> 
    <span data-ttu-id="e4055-203">V **URL posílá zaměstnanec** seznamu, můžete zvolit z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="e4055-203">In the **URL Sent to Employee** list, you can choose between the following options:</span></span>
   
    <span data-ttu-id="e4055-204">**Adresa URL jednotného přihlašování**</span><span class="sxs-lookup"><span data-stu-id="e4055-204">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="e4055-205">Systém odešle jenom adresu URL normální systému zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="e4055-205">The system sends only the normal system URL to the employee.</span></span> <span data-ttu-id="e4055-206">Zaměstnanec nemůže přihlášení pomocí jednotného přihlašování a musí používat heslo nebo místo toho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e4055-206">The employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="e4055-207">**ADRESA URL JEDNOTNÉHO PŘIHLAŠOVÁNÍ**</span><span class="sxs-lookup"><span data-stu-id="e4055-207">**SSO URL**</span></span> 
   
    <span data-ttu-id="e4055-208">Systém odešle zaměstnanec pouze adresy URL pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e4055-208">The system sends only the SSO URL to the employee.</span></span> <span data-ttu-id="e4055-209">Zaměstnanec může přihlásit pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e4055-209">The employee can log on using SSO.</span></span> <span data-ttu-id="e4055-210">Prostřednictvím rozšíření IdP přesměruje požadavek na ověření.</span><span class="sxs-lookup"><span data-stu-id="e4055-210">Authentication request is redirected through the IdP.</span></span>
   
    <span data-ttu-id="e4055-211">**Automatický výběr**</span><span class="sxs-lookup"><span data-stu-id="e4055-211">**Automatic Selection**</span></span>
   
    <span data-ttu-id="e4055-212">Pokud jednotného přihlašování není aktivní, odešle systému zaměstnanec adresu URL normální systému.</span><span class="sxs-lookup"><span data-stu-id="e4055-212">If SSO is not active, the system sends the normal system URL to the employee.</span></span> <span data-ttu-id="e4055-213">Pokud je aktivní jednotné přihlašování, systém kontroluje, zda zaměstnanec má heslo.</span><span class="sxs-lookup"><span data-stu-id="e4055-213">If SSO is active, the system checks whether the employee has a password.</span></span> <span data-ttu-id="e4055-214">Pokud heslo je k dispozici, jednotného přihlašování k URL a adresy URL bez jednotného přihlašování k odešlou do jednotlivých zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="e4055-214">If a password is available, both SSO URL and Non-SSO URL are sent to the employee.</span></span> <span data-ttu-id="e4055-215">Ale pokud zaměstnanec, nemá žádné heslo, pouze adresy URL pro jednotné přihlašování odešle zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="e4055-215">However, if the employee has no password, only the SSO URL is sent to the employee.</span></span>
   
    <span data-ttu-id="e4055-216">kB.</span><span class="sxs-lookup"><span data-stu-id="e4055-216">k.</span></span> <span data-ttu-id="e4055-217">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="e4055-217">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="e4055-218">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e4055-218">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e4055-219">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e4055-219">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e4055-220">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e4055-220">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e4055-221">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4055-221">Creating an Azure AD test user</span></span>
<span data-ttu-id="e4055-222">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4055-222">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e4055-224">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4055-224">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e4055-225">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e4055-225">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e4055-227">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e4055-227">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e4055-229">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4055-229">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e4055-231">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e4055-231">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e4055-233">a.</span><span class="sxs-lookup"><span data-stu-id="e4055-233">a.</span></span> <span data-ttu-id="e4055-234">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e4055-234">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e4055-235">b.</span><span class="sxs-lookup"><span data-stu-id="e4055-235">b.</span></span> <span data-ttu-id="e4055-236">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e4055-236">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e4055-237">c.</span><span class="sxs-lookup"><span data-stu-id="e4055-237">c.</span></span> <span data-ttu-id="e4055-238">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e4055-238">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e4055-239">d.</span><span class="sxs-lookup"><span data-stu-id="e4055-239">d.</span></span> <span data-ttu-id="e4055-240">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e4055-240">Click **Create**.</span></span>
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a><span data-ttu-id="e4055-241">Vytvoření cloudu SAP pro zákazníka testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e4055-241">Creating a SAP Cloud for Customer test user</span></span>

<span data-ttu-id="e4055-242">V této části vytvoříte uživatele volat Britta Simon v cloudu SAP pro zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e4055-242">In this section, you create a user called Britta Simon in SAP Cloud for Customer.</span></span> <span data-ttu-id="e4055-243">Spojte se s [SAP Cloud pro tým podpory zákazníků](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) přidejte uživatele v cloudu SAP pro platformu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e4055-243">Please work with [SAP Cloud for Customer support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) to add the users in the SAP Cloud for Customer platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="e4055-244">Zkontrolujte, zda hodnota NameID by měl odpovídat pole uživatelské jméno v cloudu SAP pro platformu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e4055-244">Please make sure that NameID value should match with the username field in the SAP Cloud for Customer platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e4055-245">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4055-245">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e4055-246">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu do cloudu SAP pro zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e4055-246">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAP Cloud for Customer.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e4055-248">**Přiřadit Britta Simon SAP cloudu pro zákazníka, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e4055-248">**To assign Britta Simon to SAP Cloud for Customer, perform the following steps:**</span></span>

1. <span data-ttu-id="e4055-249">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e4055-249">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e4055-251">V seznamu aplikací vyberte **SAP cloudu pro zákazníka**.</span><span class="sxs-lookup"><span data-stu-id="e4055-251">In the applications list, select **SAP Cloud for Customer**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. <span data-ttu-id="e4055-253">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e4055-253">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e4055-255">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e4055-255">Click **Add** button.</span></span> <span data-ttu-id="e4055-256">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4055-256">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e4055-258">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e4055-258">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e4055-259">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4055-259">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e4055-260">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e4055-260">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e4055-261">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e4055-261">Testing single sign-on</span></span>

<span data-ttu-id="e4055-262">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e4055-262">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e4055-263">Po kliknutí na tlačítko SAP cloudu pro dlaždice zákazníka na přístupovém panelu, jste měli získat automaticky přihlášení k SAP cloudu pro aplikace pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="e4055-263">When you click the SAP Cloud for Customer tile in the Access Panel, you should get automatically signed-on to your SAP Cloud for Customer application.</span></span>
<span data-ttu-id="e4055-264">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e4055-264">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4055-265">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e4055-265">Additional resources</span></span>

* [<span data-ttu-id="e4055-266">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e4055-266">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e4055-267">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e4055-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

