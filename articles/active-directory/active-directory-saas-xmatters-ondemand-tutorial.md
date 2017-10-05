---
title: 'Kurz: Azure Active Directory integrace s xMatters OnDemand | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a xMatters OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 9bfcb44ed19f167872b3cd9119e2dbdd35c82604
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a><span data-ttu-id="a28f7-103">Kurz: Azure Active Directory integrace s xMatters OnDemand</span><span class="sxs-lookup"><span data-stu-id="a28f7-103">Tutorial: Azure Active Directory integration with xMatters OnDemand</span></span>

<span data-ttu-id="a28f7-104">V tomto kurzu zjistěte, jak integrovat xMatters OnDemand s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a28f7-104">In this tutorial, you learn how to integrate xMatters OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a28f7-105">Integrace xMatters OnDemand s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a28f7-105">Integrating xMatters OnDemand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a28f7-106">Můžete řídit ve službě Azure AD, který má přístup k xMatters OnDemand</span><span class="sxs-lookup"><span data-stu-id="a28f7-106">You can control in Azure AD who has access to xMatters OnDemand</span></span>
- <span data-ttu-id="a28f7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k xMatters OnDemand (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a28f7-107">You can enable your users to automatically get signed-on to xMatters OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a28f7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a28f7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a28f7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a28f7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a28f7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a28f7-110">Prerequisites</span></span>

<span data-ttu-id="a28f7-111">Konfigurace integrace Azure AD s xMatters OnDemand, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a28f7-111">To configure Azure AD integration with xMatters OnDemand, you need the following items:</span></span>

- <span data-ttu-id="a28f7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a28f7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a28f7-113">Předplatné povolené xMatters OnDemand jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a28f7-113">A xMatters OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a28f7-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a28f7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a28f7-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a28f7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a28f7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a28f7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a28f7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a28f7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a28f7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a28f7-118">Scenario description</span></span>
<span data-ttu-id="a28f7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a28f7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a28f7-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a28f7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a28f7-121">Přidání xMatters OnDemand z Galerie</span><span class="sxs-lookup"><span data-stu-id="a28f7-121">Adding xMatters OnDemand from the gallery</span></span>
2. <span data-ttu-id="a28f7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a28f7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-xmatters-ondemand-from-the-gallery"></a><span data-ttu-id="a28f7-123">Přidání xMatters OnDemand z Galerie</span><span class="sxs-lookup"><span data-stu-id="a28f7-123">Adding xMatters OnDemand from the gallery</span></span>
<span data-ttu-id="a28f7-124">Při konfiguraci integrace xMatters OnDemand do služby Azure AD potřebujete přidat xMatters OnDemand z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a28f7-124">To configure the integration of xMatters OnDemand into Azure AD, you need to add xMatters OnDemand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a28f7-125">**Pokud chcete přidat xMatters OnDemand z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a28f7-125">**To add xMatters OnDemand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a28f7-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a28f7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a28f7-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a28f7-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a28f7-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a28f7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a28f7-133">Do vyhledávacího pole zadejte **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-133">In the search box, type **xMatters OnDemand**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. <span data-ttu-id="a28f7-135">Na panelu výsledků vyberte **xMatters OnDemand**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a28f7-135">In the results panel, select **xMatters OnDemand**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a28f7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a28f7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a28f7-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s xMatters OnDemand založené na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a28f7-138">In this section, you configure and test Azure AD single sign-on with xMatters OnDemand based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a28f7-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, jaké příslušného uživatele v xMatters OnDemand je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a28f7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in xMatters OnDemand is to a user in Azure AD.</span></span> <span data-ttu-id="a28f7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v xMatters OnDemand musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="a28f7-140">In other words, a link relationship between an Azure AD user and the related user in xMatters OnDemand needs to be established.</span></span>

<span data-ttu-id="a28f7-141">V xMatters OnDemand přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a28f7-141">In xMatters OnDemand, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a28f7-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s xMatters OnDemand, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a28f7-142">To configure and test Azure AD single sign-on with xMatters OnDemand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a28f7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a28f7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a28f7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a28f7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a28f7-145">**[Vytvoření zkušebního uživatele OnDemand xMatters](#creating-a-xmatters-ondemand-test-user)**  – Pokud chcete mít protějšek Britta Simon v xMatters OnDemand propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a28f7-145">**[Creating a xMatters OnDemand test user](#creating-a-xmatters-ondemand-test-user)** - to have a counterpart of Britta Simon in xMatters OnDemand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a28f7-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a28f7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a28f7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a28f7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a28f7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a28f7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a28f7-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší xMatters OnDemand aplikace.</span><span class="sxs-lookup"><span data-stu-id="a28f7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your xMatters OnDemand application.</span></span>

<span data-ttu-id="a28f7-150">**Ke konfiguraci Azure AD jednotné přihlašování s xMatters OnDemand, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a28f7-150">**To configure Azure AD single sign-on with xMatters OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="a28f7-151">Na portálu Azure na **xMatters OnDemand** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-151">In the Azure portal, on the **xMatters OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a28f7-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a28f7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. <span data-ttu-id="a28f7-155">Na **xMatters OnDemand domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a28f7-155">On the **xMatters OnDemand Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    <span data-ttu-id="a28f7-157">a.</span><span class="sxs-lookup"><span data-stu-id="a28f7-157">a.</span></span> <span data-ttu-id="a28f7-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a28f7-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    <span data-ttu-id="a28f7-159">b.</span><span class="sxs-lookup"><span data-stu-id="a28f7-159">b.</span></span> <span data-ttu-id="a28f7-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a28f7-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > <span data-ttu-id="a28f7-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a28f7-161">These values are not real.</span></span> <span data-ttu-id="a28f7-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a28f7-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a28f7-163">Obraťte se na [tým podpory xMatters OnDemand](https://www.xmatters.com/company/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a28f7-163">Contact [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/) to get these values.</span></span>

4. <span data-ttu-id="a28f7-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu místně jako **c:\\XMatters OnDemand.cer**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file locally as **c:\\XMatters OnDemand.cer**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="a28f7-166">Je třeba předávat certifikát, který chcete [tým podpory xMatters OnDemand](https://www.xmatters.com/company/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="a28f7-166">You need to forward the certificate to the [xMatters OnDemand support team](https://www.xmatters.com/company/contact-us/).</span></span> <span data-ttu-id="a28f7-167">Certifikát musí být odeslaný tým podpory xMatters předtím, než může dokončení konfigurace přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a28f7-167">The certificate needs to be uploaded by the xMatters support team before you can finalize the single sign-on configuration.</span></span> 

5. <span data-ttu-id="a28f7-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a28f7-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a28f7-170">Na **xMatters OnDemand konfigurace** klikněte na tlačítko **konfigurace xMatters OnDemand** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a28f7-170">On the **xMatters OnDemand Configuration** section, click **Configure xMatters OnDemand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a28f7-171">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a28f7-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. <span data-ttu-id="a28f7-173">V okně prohlížeče jiný web Přihlaste se na váš web společnosti XMatters OnDemand jako správce.</span><span class="sxs-lookup"><span data-stu-id="a28f7-173">In a different web browser window, log in to your XMatters OnDemand company site as an administrator.</span></span>

8. <span data-ttu-id="a28f7-174">Na panelu nástrojů v horní části klikněte na tlačítko **správce**a potom klikněte na **podrobnosti o společnosti** na navigačním panelu na levé straně.</span><span class="sxs-lookup"><span data-stu-id="a28f7-174">In the toolbar on the top, click **Admin**, and then click **Company Details** in the navigation bar on the left side.</span></span>
   
    <span data-ttu-id="a28f7-175">![Správce](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "správce")</span><span class="sxs-lookup"><span data-stu-id="a28f7-175">![Admin](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")</span></span>

9. <span data-ttu-id="a28f7-176">Na **konfigurace SAML** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a28f7-176">On the **SAML Configuration** page, perform the following steps:</span></span>
   
    <span data-ttu-id="a28f7-177">![Konfigurace SAML](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="a28f7-177">![SAML configuration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")</span></span>
   
    <span data-ttu-id="a28f7-178">a.</span><span class="sxs-lookup"><span data-stu-id="a28f7-178">a.</span></span> <span data-ttu-id="a28f7-179">Vyberte **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-179">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="a28f7-180">b.</span><span class="sxs-lookup"><span data-stu-id="a28f7-180">b.</span></span> <span data-ttu-id="a28f7-181">Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do **ID zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a28f7-181">Paste **SAML Entity ID**, which you have copied from the Azure portal into the **Identity Provider ID** textbox.</span></span>
   
    <span data-ttu-id="a28f7-182">c.</span><span class="sxs-lookup"><span data-stu-id="a28f7-182">c.</span></span> <span data-ttu-id="a28f7-183">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do **jeden přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a28f7-183">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Single Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="a28f7-184">d.</span><span class="sxs-lookup"><span data-stu-id="a28f7-184">d.</span></span> <span data-ttu-id="a28f7-185">Vložení **Sign-Out URL**, který jste zkopírovali z portálu Azure do **jednu adresu URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a28f7-185">Paste **Sign-Out URL**, which you have copied from the Azure portal into the **Single Logout URL** textbox.</span></span>
   
    <span data-ttu-id="a28f7-186">e.</span><span class="sxs-lookup"><span data-stu-id="a28f7-186">e.</span></span> <span data-ttu-id="a28f7-187">Na stránce Podrobnosti o společnosti v horní části, klikněte na **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-187">On the Company Details page, at the top, click **Save Changes**.</span></span>
    
    <span data-ttu-id="a28f7-188">![Podrobnosti o společnosti](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "společnosti podrobnosti")</span><span class="sxs-lookup"><span data-stu-id="a28f7-188">![Company details](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")</span></span>

> [!TIP]
> <span data-ttu-id="a28f7-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a28f7-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a28f7-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a28f7-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a28f7-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a28f7-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a28f7-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a28f7-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="a28f7-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a28f7-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a28f7-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a28f7-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a28f7-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a28f7-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a28f7-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a28f7-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a28f7-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a28f7-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a28f7-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a28f7-204">a.</span><span class="sxs-lookup"><span data-stu-id="a28f7-204">a.</span></span> <span data-ttu-id="a28f7-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a28f7-206">b.</span><span class="sxs-lookup"><span data-stu-id="a28f7-206">b.</span></span> <span data-ttu-id="a28f7-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a28f7-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a28f7-208">c.</span><span class="sxs-lookup"><span data-stu-id="a28f7-208">c.</span></span> <span data-ttu-id="a28f7-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a28f7-210">d.</span><span class="sxs-lookup"><span data-stu-id="a28f7-210">d.</span></span> <span data-ttu-id="a28f7-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-211">Click **Create**.</span></span>
 
### <a name="creating-a-xmatters-ondemand-test-user"></a><span data-ttu-id="a28f7-212">Vytvoření zkušebního uživatele OnDemand xMatters</span><span class="sxs-lookup"><span data-stu-id="a28f7-212">Creating a xMatters OnDemand test user</span></span>

<span data-ttu-id="a28f7-213">Pokud chcete povolit uživatelům Azure AD přihlášení k XMatters OnDemand, musí být zřízená do XMatters OnDemand.</span><span class="sxs-lookup"><span data-stu-id="a28f7-213">In order to enable Azure AD users to log in to XMatters OnDemand, they must be provisioned into XMatters OnDemand.</span></span> <span data-ttu-id="a28f7-214">V případě XMatters OnDemand zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a28f7-214">In the case of XMatters OnDemand, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="a28f7-215">Ke zřízení uživatelských účtů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a28f7-215">To provision a user accounts, perform the following steps:</span></span>
1. <span data-ttu-id="a28f7-216">Přihlaste se k vaší **XMatters OnDemand** klienta.</span><span class="sxs-lookup"><span data-stu-id="a28f7-216">Log in to your **XMatters OnDemand** tenant.</span></span>

2.  <span data-ttu-id="a28f7-217">Klikněte na tlačítko **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="a28f7-217">Click **Users** tab.</span></span> <span data-ttu-id="a28f7-218">a pak klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-218">and then click **Add User**.</span></span>
  
    <span data-ttu-id="a28f7-219">![Uživatelé](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a28f7-219">![Users](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")</span></span>

3. <span data-ttu-id="a28f7-220">V **přidat uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a28f7-220">In the **Add a User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="a28f7-221">![Přidat uživatele](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="a28f7-221">![Add a User](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")</span></span>

    <span data-ttu-id="a28f7-222">a.</span><span class="sxs-lookup"><span data-stu-id="a28f7-222">a.</span></span> <span data-ttu-id="a28f7-223">Vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-223">Select **Active**.</span></span>

    <span data-ttu-id="a28f7-224">b.</span><span class="sxs-lookup"><span data-stu-id="a28f7-224">b.</span></span> <span data-ttu-id="a28f7-225">V **ID uživatele** jako typ id uživatele uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="a28f7-225">In the **User ID** textbox, type the user id of user like Brittasimon@contoso.com.</span></span>
   
    <span data-ttu-id="a28f7-226">c.</span><span class="sxs-lookup"><span data-stu-id="a28f7-226">c.</span></span> <span data-ttu-id="a28f7-227">V **křestní jméno** textovému poli, typ křestní jméno uživatele, jako je Britta.</span><span class="sxs-lookup"><span data-stu-id="a28f7-227">In the **First Name** textbox, type first name of the user like Britta.</span></span>

    <span data-ttu-id="a28f7-228">d.</span><span class="sxs-lookup"><span data-stu-id="a28f7-228">d.</span></span> <span data-ttu-id="a28f7-229">V **příjmení** textovému poli, zadejte příjmení uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="a28f7-229">In the **Last Name** textbox, type last name of the user like Simon.</span></span>
    
    <span data-ttu-id="a28f7-230">e.</span><span class="sxs-lookup"><span data-stu-id="a28f7-230">e.</span></span> <span data-ttu-id="a28f7-231">V **lokality** textovému poli, zadejte platnou lokalitou platný Azure AD účet chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="a28f7-231">In the **Site** textbox, Enter the valid site of a valid Azure AD account you want to provision.</span></span>
    
    <span data-ttu-id="a28f7-232">f.</span><span class="sxs-lookup"><span data-stu-id="a28f7-232">f.</span></span> <span data-ttu-id="a28f7-233">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-233">Click **Save**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a28f7-234">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a28f7-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a28f7-235">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k xMatters OnDemand.</span><span class="sxs-lookup"><span data-stu-id="a28f7-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to xMatters OnDemand.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a28f7-237">**Pokud chcete přiřadit Britta Simon xMatters OnDemand, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a28f7-237">**To assign Britta Simon to xMatters OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="a28f7-238">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a28f7-240">V seznamu aplikací vyberte **xMatters OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-240">In the applications list, select **xMatters OnDemand**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. <span data-ttu-id="a28f7-242">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a28f7-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a28f7-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a28f7-244">Click **Add** button.</span></span> <span data-ttu-id="a28f7-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a28f7-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a28f7-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a28f7-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a28f7-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a28f7-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a28f7-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a28f7-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a28f7-250">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a28f7-250">Testing single sign-on</span></span>

<span data-ttu-id="a28f7-251">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a28f7-251">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a28f7-252">Po kliknutí na tlačítko xMatters OnDemand dlaždice na přístupovém panelu, jste měli získat automaticky přihlášení k vaší xMatters OnDemand aplikace.</span><span class="sxs-lookup"><span data-stu-id="a28f7-252">When you click the xMatters OnDemand tile in the Access Panel, you should get automatically signed-on to your xMatters OnDemand application.</span></span>
<span data-ttu-id="a28f7-253">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a28f7-253">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a28f7-254">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a28f7-254">Additional resources</span></span>

* [<span data-ttu-id="a28f7-255">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a28f7-255">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a28f7-256">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a28f7-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png
