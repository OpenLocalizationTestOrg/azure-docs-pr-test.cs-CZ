---
title: 'Kurz: Azure Active Directory integrace s Enterprise smysl Qlik | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Qlik smysl Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 4964634cd5aaf0dbb98c766f5e12700c4d118750
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a><span data-ttu-id="94d36-103">Kurz: Azure Active Directory integrace s Qlik smysl Enterprise</span><span class="sxs-lookup"><span data-stu-id="94d36-103">Tutorial: Azure Active Directory integration with Qlik Sense Enterprise</span></span>

<span data-ttu-id="94d36-104">V tomto kurzu zjistěte, jak integrovat Qlik smysl Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="94d36-104">In this tutorial, you learn how to integrate Qlik Sense Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="94d36-105">Integrace Qlik smysl Enterprise s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="94d36-105">Integrating Qlik Sense Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="94d36-106">Můžete ovládat ve službě Azure AD, který má přístup do firemní sítě Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-106">You can control in Azure AD who has access to Qlik Sense Enterprise.</span></span>
- <span data-ttu-id="94d36-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Qlik smysl Enterprise (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94d36-107">You can enable your users to automatically get signed-on to Qlik Sense Enterprise (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="94d36-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="94d36-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="94d36-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="94d36-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94d36-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="94d36-110">Prerequisites</span></span>

<span data-ttu-id="94d36-111">Konfigurace integrace Azure AD s Qlik smysl Enterprise, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="94d36-111">To configure Azure AD integration with Qlik Sense Enterprise, you need the following items:</span></span>

- <span data-ttu-id="94d36-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="94d36-112">An Azure AD subscription</span></span>
- <span data-ttu-id="94d36-113">Qlik smysl Enterprise jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="94d36-113">A Qlik Sense Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="94d36-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="94d36-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="94d36-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="94d36-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="94d36-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="94d36-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="94d36-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="94d36-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="94d36-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="94d36-118">Scenario description</span></span>
<span data-ttu-id="94d36-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="94d36-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="94d36-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="94d36-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="94d36-121">Přidání Qlik smysl Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="94d36-121">Adding Qlik Sense Enterprise from the gallery</span></span>
2. <span data-ttu-id="94d36-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="94d36-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a><span data-ttu-id="94d36-123">Přidání Qlik smysl Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="94d36-123">Adding Qlik Sense Enterprise from the gallery</span></span>
<span data-ttu-id="94d36-124">Při konfiguraci integrace Qlik smysl Enterprise do služby Azure AD potřebujete přidat Qlik smysl Enterprise z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="94d36-124">To configure the integration of Qlik Sense Enterprise into Azure AD, you need to add Qlik Sense Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="94d36-125">**Pokud chcete přidat Qlik smysl Enterprise z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="94d36-125">**To add Qlik Sense Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="94d36-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="94d36-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="94d36-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="94d36-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="94d36-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="94d36-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="94d36-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="94d36-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="94d36-133">Do vyhledávacího pole zadejte **Qlik smysl Enterprise**, vyberte **Qlik smysl Enterprise** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="94d36-133">In the search box, type **Qlik Sense Enterprise**, select **Qlik Sense Enterprise** from result panel then click **Add** button to add the application.</span></span>

    ![Enterprise Qlik smysl v seznamu výsledků](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="94d36-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="94d36-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="94d36-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Enterprise smysl Qlik podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="94d36-136">In this section, you configure and test Azure AD single sign-on with Qlik Sense Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="94d36-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Qlik smysl Enterprise je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94d36-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Qlik Sense Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="94d36-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v podniku Qlik smysl, je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="94d36-138">In other words, a link relationship between an Azure AD user and the related user in Qlik Sense Enterprise needs to be established.</span></span>

<span data-ttu-id="94d36-139">V Qlik smysl Enterprise, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="94d36-139">In Qlik Sense Enterprise, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="94d36-140">Nakonfigurovat a otestovat Azure AD jednotného přihlašování k Qlik smysl organizace, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="94d36-140">To configure and test Azure AD single sign-on with Qlik Sense Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="94d36-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="94d36-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="94d36-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="94d36-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="94d36-143">**[Vytvoření zkušebního uživatele Qlik smysl Enterprise](#create-a-qlik-sense-enterprise-test-user)**  – Pokud chcete mít protějšek Britta Simon v Qlik smysl organizace, která je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="94d36-143">**[Create a Qlik Sense Enterprise test user](#create-a-qlik-sense-enterprise-test-user)** - to have a counterpart of Britta Simon in Qlik Sense Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="94d36-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="94d36-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="94d36-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94d36-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="94d36-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="94d36-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="94d36-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Qlik smysl Enterprise.</span><span class="sxs-lookup"><span data-stu-id="94d36-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Qlik Sense Enterprise application.</span></span>

<span data-ttu-id="94d36-148">**Ke konfiguraci Azure AD jednotného přihlašování k Qlik smysl organizace, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="94d36-148">**To configure Azure AD single sign-on with Qlik Sense Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="94d36-149">Na portálu Azure na **Qlik smysl Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="94d36-149">In the Azure portal, on the **Qlik Sense Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="94d36-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="94d36-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. <span data-ttu-id="94d36-153">Na **Qlik smysl podnikové domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="94d36-153">On the **Qlik Sense Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![Qlik smysl podnikové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    <span data-ttu-id="94d36-155">a.</span><span class="sxs-lookup"><span data-stu-id="94d36-155">a.</span></span> <span data-ttu-id="94d36-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span><span class="sxs-lookup"><span data-stu-id="94d36-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="94d36-157">Všimněte si do adresy koncové lomítko na konci tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="94d36-157">Note the trailing slash at the end of this URI.</span></span> <span data-ttu-id="94d36-158">Je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="94d36-158">It is required.</span></span>
    
    <span data-ttu-id="94d36-159">b.</span><span class="sxs-lookup"><span data-stu-id="94d36-159">b.</span></span> <span data-ttu-id="94d36-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="94d36-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > <span data-ttu-id="94d36-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="94d36-161">These values are not real.</span></span> <span data-ttu-id="94d36-162">Tyto hodnoty aktualizovat se skutečné přihlašovací adresa URL a identifikátor, který je popsán později v tomto kurzu nebo kontaktujte [tým podpory klient systému Enterprise smysl Qlik](https://www.qlik.com/us/services/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="94d36-162">Update these values with the actual Sign-On URL and Identifier, Which are explained later in this tutorial or contact [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to get these values.</span></span> 

4. <span data-ttu-id="94d36-163">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="94d36-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. <span data-ttu-id="94d36-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-165">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="94d36-167">Příprava souboru XML federační Metadata tak, aby, můžete nahrát na server Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-167">Prepare the Federation Metadata XML file so that you can upload that to Qlik Sense server.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="94d36-168">Před nahráním IdP metadata k serveru Qlik smysl, je třeba soubor upravit, chcete-li odebrat informace, které zajistit správnou funkci mezi službou Azure AD a server Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-168">Before uploading the IdP metadata to the Qlik Sense server, the file needs to be edited to remove information to ensure proper operation between Azure AD and Qlik Sense server.</span></span>
    
    ![QlikSense][qs24]
   
    <span data-ttu-id="94d36-170">a.</span><span class="sxs-lookup"><span data-stu-id="94d36-170">a.</span></span> <span data-ttu-id="94d36-171">Otevřete soubor FederationMetaData.xml, který jste si stáhli z portálu Azure v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="94d36-171">Open the FederationMetaData.xml file, which you have downloaded from Azure portal in a text editor.</span></span>
   
    <span data-ttu-id="94d36-172">b.</span><span class="sxs-lookup"><span data-stu-id="94d36-172">b.</span></span> <span data-ttu-id="94d36-173">Vyhledejte hodnotu **RoleDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="94d36-173">Search for the value **RoleDescriptor**.</span></span>  <span data-ttu-id="94d36-174">Existují čtyři záznamy (dvě dvojice otevření a zavření značky elementu).</span><span class="sxs-lookup"><span data-stu-id="94d36-174">There are four entries (two pairs of opening and closing element tags).</span></span>
   
    <span data-ttu-id="94d36-175">c.</span><span class="sxs-lookup"><span data-stu-id="94d36-175">c.</span></span> <span data-ttu-id="94d36-176">Odstraníte RoleDescriptor značky a všechny informace v rozmezí ze souboru.</span><span class="sxs-lookup"><span data-stu-id="94d36-176">Delete the RoleDescriptor tags and all information in between from the file.</span></span>
   
    <span data-ttu-id="94d36-177">d.</span><span class="sxs-lookup"><span data-stu-id="94d36-177">d.</span></span> <span data-ttu-id="94d36-178">Uložte soubor a uložte nedaleko pro použití později v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="94d36-178">Save the file and keep it nearby for use later in this document.</span></span>

7. <span data-ttu-id="94d36-179">Přejděte k Qlik smysl Qlik Management Console (QMC) jako uživatel, který můžete vytvořit virtuální proxy konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94d36-179">Navigate to the Qlik Sense Qlik Management Console (QMC) as a user who can create virtual proxy configurations.</span></span>

8. <span data-ttu-id="94d36-180">V QMC, klikněte na **virtuální proxy** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="94d36-180">In the QMC, click on the **Virtual Proxies** menu item.</span></span>
   
    ![QlikSense][qs6] 

9. <span data-ttu-id="94d36-182">V dolní části obrazovky klepněte **vytvořit nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-182">At the bottom of the screen, click the **Create new** button.</span></span>
   
    ![QlikSense][qs7]

10. <span data-ttu-id="94d36-184">Zobrazí se obrazovka virtuální proxy upravit.</span><span class="sxs-lookup"><span data-stu-id="94d36-184">The Virtual proxy edit screen appears.</span></span>  <span data-ttu-id="94d36-185">Na pravé straně obrazovky je nabídka pro viditelnosti možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="94d36-185">On the right side of the screen is a menu for making configuration options visible.</span></span>
   
    ![QlikSense][qs9]

11. <span data-ttu-id="94d36-187">S možností nabídky identifikace zaškrtnuto zadejte identifikační informace pro konfiguraci Azure virtuální proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="94d36-187">With the Identification menu option checked, enter the identifying information for the Azure virtual proxy configuration.</span></span>
    
    ![QlikSense][qs8]  
    
    <span data-ttu-id="94d36-189">a.</span><span class="sxs-lookup"><span data-stu-id="94d36-189">a.</span></span> <span data-ttu-id="94d36-190">**Popis** pole je popisný název pro konfiguraci virtuálního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="94d36-190">The **Description** field is a friendly name for the virtual proxy configuration.</span></span>  <span data-ttu-id="94d36-191">Zadejte hodnotu pro popis.</span><span class="sxs-lookup"><span data-stu-id="94d36-191">Enter a value for a description.</span></span>
    
    <span data-ttu-id="94d36-192">b.</span><span class="sxs-lookup"><span data-stu-id="94d36-192">b.</span></span> <span data-ttu-id="94d36-193">**Předpony** pole identifikuje koncový bod virtuálního proxy pro připojení k Qlik smysl s Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="94d36-193">The **Prefix** field identifies the virtual proxy endpoint for connecting to Qlik Sense with Azure AD Single Sign-On.</span></span>  <span data-ttu-id="94d36-194">Zadejte název jedinečnou předponu pro tento virtuální server proxy.</span><span class="sxs-lookup"><span data-stu-id="94d36-194">Enter a unique prefix name for this virtual proxy.</span></span>
    
    <span data-ttu-id="94d36-195">c.</span><span class="sxs-lookup"><span data-stu-id="94d36-195">c.</span></span> <span data-ttu-id="94d36-196">**Časový limit nečinnosti relace (minuty)** vypršel časový limit pro připojení přes tuto virtuální proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="94d36-196">**Session inactivity timeout (minutes)** is the timeout for connections through this virtual proxy.</span></span>
    
    <span data-ttu-id="94d36-197">d.</span><span class="sxs-lookup"><span data-stu-id="94d36-197">d.</span></span> <span data-ttu-id="94d36-198">**Název hlavičky souboru cookie relace** je název souboru cookie pro relaci Qlik smysl uživatel obdrží po úspěšném ověření ukládání identifikátor relace.</span><span class="sxs-lookup"><span data-stu-id="94d36-198">The **Session cookie header name** is the cookie name storing the session identifier for the Qlik Sense session a user receives after successful authentication.</span></span>  <span data-ttu-id="94d36-199">Tento název musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="94d36-199">This name must be unique.</span></span>

12. <span data-ttu-id="94d36-200">Klikněte na možnost nabídky ověřování vytvořit viditelné.</span><span class="sxs-lookup"><span data-stu-id="94d36-200">Click on the Authentication menu option to make it visible.</span></span>  <span data-ttu-id="94d36-201">Zobrazí se obrazovka ověřování.</span><span class="sxs-lookup"><span data-stu-id="94d36-201">The Authentication screen appears.</span></span>
    
    ![QlikSense][qs10]
    
    <span data-ttu-id="94d36-203">a.</span><span class="sxs-lookup"><span data-stu-id="94d36-203">a.</span></span> <span data-ttu-id="94d36-204">**Anonymní přístup režimu** rozevíracího seznamu určuje Pokud anonymní uživatelé mohou přistupovat k Qlik smysl přes virtuální proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="94d36-204">The **Anonymous access mode** drop down determines if anonymous users may access Qlik Sense through the virtual proxy.</span></span>  <span data-ttu-id="94d36-205">Výchozí nastavení je anonymní uživatel.</span><span class="sxs-lookup"><span data-stu-id="94d36-205">The default option is No anonymous user.</span></span>
    
    <span data-ttu-id="94d36-206">b.</span><span class="sxs-lookup"><span data-stu-id="94d36-206">b.</span></span> <span data-ttu-id="94d36-207">**Metodu ověřování** rozevíracího seznamu určuje schéma ověřování proxy virtuální použije.</span><span class="sxs-lookup"><span data-stu-id="94d36-207">The **Authentication method** drop-down determines the authentication scheme the virtual proxy will use.</span></span>  <span data-ttu-id="94d36-208">V rozevíracím seznamu vyberte SAML.</span><span class="sxs-lookup"><span data-stu-id="94d36-208">Select SAML from the drop-down list.</span></span>  <span data-ttu-id="94d36-209">Proto se zobrazí další možnosti.</span><span class="sxs-lookup"><span data-stu-id="94d36-209">More options appear as a result.</span></span>
    
    <span data-ttu-id="94d36-210">c.</span><span class="sxs-lookup"><span data-stu-id="94d36-210">c.</span></span> <span data-ttu-id="94d36-211">V **pole URI hostitel SAML**, zadejte název hostitele uživatele zadejte přístup k Qlik smysl prostřednictvím tento proxy server virtuální SAML.</span><span class="sxs-lookup"><span data-stu-id="94d36-211">In the **SAML host URI field**, input the hostname users enter to access Qlik Sense through this SAML virtual proxy.</span></span>  <span data-ttu-id="94d36-212">Název hostitele je identifikátor uri serveru Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-212">The hostname is the uri of the Qlik Sense server.</span></span>
    
    <span data-ttu-id="94d36-213">d.</span><span class="sxs-lookup"><span data-stu-id="94d36-213">d.</span></span> <span data-ttu-id="94d36-214">V **SAML entity ID**, zadejte stejné hodnota zadaná pro pole SAML hostitel identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="94d36-214">In the **SAML entity ID**, enter the same value entered for the SAML host URI field.</span></span>
    
    <span data-ttu-id="94d36-215">e.</span><span class="sxs-lookup"><span data-stu-id="94d36-215">e.</span></span> <span data-ttu-id="94d36-216">**SAML IdP metadata** je soubor upravit dříve v **upravit federačních metadat z konfigurace služby Azure AD** části.</span><span class="sxs-lookup"><span data-stu-id="94d36-216">The **SAML IdP metadata** is the file edited earlier in the **Edit Federation Metadata from Azure AD Configuration** section.</span></span>  <span data-ttu-id="94d36-217">**Před nahráním IdP metadata, soubor musí být upravena** odebrat informace o pracovat správně mezi službou Azure AD a server Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-217">**Before uploading the IdP metadata, the file needs to be edited** to remove information to ensure proper operation between Azure AD and Qlik Sense server.</span></span>  <span data-ttu-id="94d36-218">**Naleznete pokyny výše, pokud má soubor ještě k provádění úprav.**</span><span class="sxs-lookup"><span data-stu-id="94d36-218">**Please refer to the instructions above if the file has yet to be edited.**</span></span>  <span data-ttu-id="94d36-219">Pokud soubor byl upraven, klikněte na tlačítko Procházet a vyberte soubor upravená metadata nahrát do konfigurace virtuálního serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="94d36-219">If the file has been edited click on the Browse button and select the edited metadata file to upload it to the virtual proxy configuration.</span></span>
    
    <span data-ttu-id="94d36-220">f.</span><span class="sxs-lookup"><span data-stu-id="94d36-220">f.</span></span> <span data-ttu-id="94d36-221">Zadejte odkaz na atribut název nebo schéma pro představující atribut SAML **UserID** Azure AD odešle na server Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-221">Enter the attribute name or schema reference for the SAML attribute representing the **UserID** Azure AD sends to the Qlik Sense server.</span></span>  <span data-ttu-id="94d36-222">Informace o schématu odkaz je k dispozici v konfiguraci aplikace Azure obrazovky post.</span><span class="sxs-lookup"><span data-stu-id="94d36-222">Schema reference information is available in the Azure app screens post configuration.</span></span>  <span data-ttu-id="94d36-223">Chcete-li použít atribut názvu, zadejte `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="94d36-223">To use the name attribute, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="94d36-224">g.</span><span class="sxs-lookup"><span data-stu-id="94d36-224">g.</span></span> <span data-ttu-id="94d36-225">Zadejte hodnotu pro **adresář uživatelského** uživatelům, který bude připojen v při ověření Qlik smysl serveru prostřednictvím služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94d36-225">Enter the value for the **user directory** that will be attached to users when they authenticate to Qlik Sense server through Azure AD.</span></span>  <span data-ttu-id="94d36-226">Pevně zakódované hodnoty musí být uzavřena do **hranaté závorky []**.</span><span class="sxs-lookup"><span data-stu-id="94d36-226">Hardcoded values must be surrounded by **square brackets []**.</span></span>  <span data-ttu-id="94d36-227">Chcete-li použít atribut odeslány ve výrazu Azure AD SAML, zadejte název atributu v tohoto textového pole **bez** hranaté závorky.</span><span class="sxs-lookup"><span data-stu-id="94d36-227">To use an attribute sent in the Azure AD SAML assertion, enter the name of the attribute in this text box **without** square brackets.</span></span>
    
    <span data-ttu-id="94d36-228">h.</span><span class="sxs-lookup"><span data-stu-id="94d36-228">h.</span></span> <span data-ttu-id="94d36-229">**SAML podpisový algoritmus** nastaví podpis pro konfiguraci proxy serveru virtuální certifikát služby zprostředkovatele (v tomto případu Qlik smysl serveru).</span><span class="sxs-lookup"><span data-stu-id="94d36-229">The **SAML signing algorithm** sets the service provider (in this case Qlik Sense server) certificate signing for the virtual proxy configuration.</span></span>  <span data-ttu-id="94d36-230">Pokud Qlik smysl serveru využívá důvěryhodné certifikát generovaný pomocí Microsoft Enhanced RSA a AES Cryptographic Provider, změňte SAML podpisový algoritmus, který se **SHA-256**.</span><span class="sxs-lookup"><span data-stu-id="94d36-230">If Qlik Sense server uses a trusted certificate generated using Microsoft Enhanced RSA and AES Cryptographic Provider, change the SAML signing algorithm to **SHA-256**.</span></span>
    
    <span data-ttu-id="94d36-231">i.</span><span class="sxs-lookup"><span data-stu-id="94d36-231">i.</span></span> <span data-ttu-id="94d36-232">V části mapování atributů SAML umožňuje další atributy, jako jsou skupiny k odeslání do Qlik smysl pro použití v pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="94d36-232">The SAML attribute mapping section allows for additional attributes like groups to be sent to Qlik Sense for use in security rules.</span></span>

13. <span data-ttu-id="94d36-233">Klikněte na **Vyrovnávání zatížení** nabídky možnost vytvořit viditelné.</span><span class="sxs-lookup"><span data-stu-id="94d36-233">Click on the **LOAD BALANCING** menu option to make it visible.</span></span>  <span data-ttu-id="94d36-234">Zobrazí se obrazovka Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="94d36-234">The Load Balancing screen appears.</span></span>
    
    ![QlikSense][qs11]

14. <span data-ttu-id="94d36-236">Klikněte na **přidat nový uzel serveru** tlačítko, vyberte modul uzlu nebo uzlů Qlik smysl odešle relací pro účely Vyrovnávání zatížení, a klikněte na **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-236">Click on the **Add new server node** button, select engine node or nodes Qlik Sense will send sessions to for load balancing purposes, and click the **Add** button.</span></span>
    
    ![QlikSense][qs12]

15. <span data-ttu-id="94d36-238">Klikněte na možnost Pokročilé nabídky zviditelnit.</span><span class="sxs-lookup"><span data-stu-id="94d36-238">Click on the Advanced menu option to make it visible.</span></span> <span data-ttu-id="94d36-239">Zobrazí se obrazovka Upřesnit.</span><span class="sxs-lookup"><span data-stu-id="94d36-239">The Advanced screen appears.</span></span>
    
    ![QlikSense][qs13]
    
    <span data-ttu-id="94d36-241">Seznamu povolených hostitele identifikuje názvy hostitelů, které jsou přijaty při připojování k serveru Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-241">The Host white list identifies hostnames that are accepted when connecting to the Qlik Sense server.</span></span>  <span data-ttu-id="94d36-242">**Zadejte název hostitele, které budou uživatelé zadávat při připojování k serveru Qlik smysl.**</span><span class="sxs-lookup"><span data-stu-id="94d36-242">**Enter the hostname users will specify when connecting to Qlik Sense server.**</span></span> <span data-ttu-id="94d36-243">Název hostitele je stejnou hodnotu jako identifikátor uri SAML hostitele bez https://.</span><span class="sxs-lookup"><span data-stu-id="94d36-243">The hostname is the same value as the SAML host uri without the https://.</span></span>

16. <span data-ttu-id="94d36-244">Klikněte **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-244">Click the **Apply** button.</span></span>
    
    ![QlikSense][qs14]

17. <span data-ttu-id="94d36-246">Kliknutím na tlačítko OK přijímat upozornění, které stavy proxy propojené s virtuální proxy server se restartuje.</span><span class="sxs-lookup"><span data-stu-id="94d36-246">Click OK to accept the warning message that states proxies linked to the virtual proxy will be restarted.</span></span>
    
    ![QlikSense][qs15]

18. <span data-ttu-id="94d36-248">Na pravé straně obrazovky zobrazí se v nabídce přidružené položky.</span><span class="sxs-lookup"><span data-stu-id="94d36-248">On the right side of the screen, the Associated items menu appears.</span></span>  <span data-ttu-id="94d36-249">Klikněte na **proxy** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="94d36-249">Click on the **Proxies** menu option.</span></span>
    
    ![QlikSense][qs16]

19. <span data-ttu-id="94d36-251">Zobrazí se obrazovka proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="94d36-251">The proxy screen appears.</span></span>  <span data-ttu-id="94d36-252">Klikněte **odkaz** tlačítko dole propojení proxy server na virtuální server proxy.</span><span class="sxs-lookup"><span data-stu-id="94d36-252">Click the **Link** button at the bottom to link a proxy to the virtual proxy.</span></span>
    
    ![QlikSense][qs17]

20. <span data-ttu-id="94d36-254">Vyberte uzel proxy serveru, který bude podporovat toto připojení virtuální proxy serveru a klikněte na **odkaz** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-254">Select the proxy node that will support this virtual proxy connection and click the **Link** button.</span></span>  <span data-ttu-id="94d36-255">Po propojení, se objeví pod přidružené proxy proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="94d36-255">After linking, the proxy will be listed under associated proxies.</span></span>
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. <span data-ttu-id="94d36-258">Po přibližně pět až deset sekund zobrazí se zpráva QMC aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="94d36-258">After about five to ten seconds, the Refresh QMC message will appear.</span></span>  <span data-ttu-id="94d36-259">Klikněte **aktualizovat QMC** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-259">Click the **Refresh QMC** button.</span></span>
    
    ![QlikSense][qs20]

22. <span data-ttu-id="94d36-261">Když QMC aktualizuje, klikněte na **virtuální proxy** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="94d36-261">When the QMC refreshes, click on the **Virtual proxies** menu item.</span></span> <span data-ttu-id="94d36-262">Nové položky virtuální proxy SAML je uvedené v tabulce na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="94d36-262">The new SAML virtual proxy entry is listed in the table on the screen.</span></span>  <span data-ttu-id="94d36-263">Jedním kliknutím na položku virtuální proxy.</span><span class="sxs-lookup"><span data-stu-id="94d36-263">Single click on the virtual proxy entry.</span></span>
    
    ![QlikSense][qs51]

23. <span data-ttu-id="94d36-265">V dolní části obrazovky se budou aktivovat tlačítko Stáhnout SP metadat.</span><span class="sxs-lookup"><span data-stu-id="94d36-265">At the bottom of the screen, the Download SP metadata button will activate.</span></span>  <span data-ttu-id="94d36-266">Klikněte **stáhnout SP metadata** tlačítko Uložit metadata do souboru.</span><span class="sxs-lookup"><span data-stu-id="94d36-266">Click the **Download SP metadata** button to save the metadata to a file.</span></span>
    
    ![QlikSense][qs52]

24. <span data-ttu-id="94d36-268">Otevřete soubor metadat poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="94d36-268">Open the sp metadata file.</span></span>  <span data-ttu-id="94d36-269">Sledovat **entityID** položku a **AssertionConsumerService** položku.</span><span class="sxs-lookup"><span data-stu-id="94d36-269">Observe the **entityID** entry and the **AssertionConsumerService** entry.</span></span>  <span data-ttu-id="94d36-270">Tyto hodnoty jsou rovnocenná **identifikátor** a **přihlásit na adrese URL** v konfiguraci aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94d36-270">These values are equivalent to the **Identifier** and the **Sign on URL** in the Azure AD application configuration.</span></span> <span data-ttu-id="94d36-271">Vložte v tyto hodnoty **Qlik smysl podnikové domény a adresy URL** část v konfiguraci aplikace Azure AD, pokud jejich nejsou párování, pak by měl nahradíte je v Průvodci konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94d36-271">Paste these values in the **Qlik Sense Enterprise Domain and URLs** section in the Azure AD application configuration if they are not matching, then you should replace them in the Azure AD App configuration wizard.</span></span>
    
    ![QlikSense][qs53]

> [!TIP]
> <span data-ttu-id="94d36-273">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="94d36-273">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="94d36-274">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="94d36-274">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="94d36-275">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="94d36-275">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="94d36-276">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="94d36-276">Create an Azure AD test user</span></span>
<span data-ttu-id="94d36-277">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="94d36-277">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="94d36-279">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="94d36-279">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="94d36-280">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-280">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

   ![Tlačítko Azure Active Directory](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="94d36-282">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="94d36-282">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

   !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="94d36-284">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="94d36-284">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

   ![Tlačítko Přidat](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="94d36-286">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="94d36-286">In the **User** dialog box, perform the following steps:</span></span>

   ![Dialogové okno uživatele](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   <span data-ttu-id="94d36-288">a.</span><span class="sxs-lookup"><span data-stu-id="94d36-288">a.</span></span> <span data-ttu-id="94d36-289">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="94d36-289">In the **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="94d36-290">b.</span><span class="sxs-lookup"><span data-stu-id="94d36-290">b.</span></span> <span data-ttu-id="94d36-291">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="94d36-291">In the **User name** box, type the email address of user Britta Simon.</span></span>

   <span data-ttu-id="94d36-292">c.</span><span class="sxs-lookup"><span data-stu-id="94d36-292">c.</span></span> <span data-ttu-id="94d36-293">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="94d36-293">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

   <span data-ttu-id="94d36-294">d.</span><span class="sxs-lookup"><span data-stu-id="94d36-294">d.</span></span> <span data-ttu-id="94d36-295">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="94d36-295">Click **Create**.</span></span>
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a><span data-ttu-id="94d36-296">Vytvoření zkušebního uživatele Qlik smysl Enterprise</span><span class="sxs-lookup"><span data-stu-id="94d36-296">Create a Qlik Sense Enterprise test user</span></span>

<span data-ttu-id="94d36-297">V této části vytvoříte uživatele volal Britta Simon v Qlik smysl Enterprise.</span><span class="sxs-lookup"><span data-stu-id="94d36-297">In this section, you create a user called Britta Simon in Qlik Sense Enterprise.</span></span> <span data-ttu-id="94d36-298">Práce s [tým podpory klient systému Enterprise smysl Qlik](https://www.qlik.com/us/services/support) přidejte uživatele v platformě Qlik smysl Enterprise.</span><span class="sxs-lookup"><span data-stu-id="94d36-298">Work with [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to add the users in the Qlik Sense Enterprise platform.</span></span> <span data-ttu-id="94d36-299">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="94d36-299">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="94d36-300">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="94d36-300">Assign the Azure AD test user</span></span>

<span data-ttu-id="94d36-301">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup do firemní sítě Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="94d36-301">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Qlik Sense Enterprise.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="94d36-303">**Pokud chcete přiřadit Britta Simon Qlik smysl Enterprise, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="94d36-303">**To assign Britta Simon to Qlik Sense Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="94d36-304">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="94d36-304">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="94d36-306">V seznamu aplikací vyberte **Qlik smysl Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="94d36-306">In the applications list, select **Qlik Sense Enterprise**.</span></span>

    ![V seznamu aplikací na odkaz Qlik smysl Enterprise](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. <span data-ttu-id="94d36-308">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="94d36-308">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="94d36-310">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d36-310">Click **Add** button.</span></span> <span data-ttu-id="94d36-311">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="94d36-311">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="94d36-313">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="94d36-313">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="94d36-314">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="94d36-314">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="94d36-315">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="94d36-315">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="94d36-316">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="94d36-316">Test single sign-on</span></span>

<span data-ttu-id="94d36-317">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="94d36-317">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="94d36-318">Když kliknete na dlaždici Qlik smysl Enterprise na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Qlik smysl Enterprise.</span><span class="sxs-lookup"><span data-stu-id="94d36-318">When you click the Qlik Sense Enterprise tile in the Access Panel, you should get automatically signed-on to your Qlik Sense Enterprise application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="94d36-319">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="94d36-319">Additional resources</span></span>

* [<span data-ttu-id="94d36-320">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="94d36-320">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="94d36-321">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="94d36-321">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

