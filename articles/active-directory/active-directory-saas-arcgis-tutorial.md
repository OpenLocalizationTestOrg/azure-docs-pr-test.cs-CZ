---
title: 'Kurz: Azure Active Directory integrace s ArcGIS Online | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ArcGIS Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: df72270ca6443b456c079b22425f1660aa522389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a><span data-ttu-id="35b0f-103">Kurz: Azure Active Directory integrace s ArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="35b0f-103">Tutorial: Azure Active Directory integration with ArcGIS Online</span></span>

<span data-ttu-id="35b0f-104">V tomto kurzu zjistěte, jak integrovat ArcGIS Online s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="35b0f-104">In this tutorial, you learn how to integrate ArcGIS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="35b0f-105">Integrace ArcGIS Online s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="35b0f-105">Integrating ArcGIS Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="35b0f-106">Můžete řídit ve službě Azure AD, který má přístup k ArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="35b0f-106">You can control in Azure AD who has access to ArcGIS Online</span></span>
- <span data-ttu-id="35b0f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ArcGIS Online (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="35b0f-107">You can enable your users to automatically get signed-on to ArcGIS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="35b0f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="35b0f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="35b0f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="35b0f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with ArcGIS Online, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="35b0f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="35b0f-110">Prerequisites</span></span>

<span data-ttu-id="35b0f-111">Konfigurace integrace Azure AD s ArcGIS Online, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="35b0f-111">To configure Azure AD integration with ArcGIS Online, you need the following items:</span></span>

- <span data-ttu-id="35b0f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="35b0f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="35b0f-113">ArcGIS Online jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="35b0f-113">A ArcGIS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="35b0f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="35b0f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="35b0f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="35b0f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="35b0f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="35b0f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="35b0f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="35b0f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="35b0f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="35b0f-118">Scenario description</span></span>
<span data-ttu-id="35b0f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="35b0f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="35b0f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="35b0f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="35b0f-121">Přidání ArcGIS Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="35b0f-121">Adding ArcGIS Online from the gallery</span></span>
2. <span data-ttu-id="35b0f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="35b0f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-arcgis-online-from-the-gallery"></a><span data-ttu-id="35b0f-123">Přidání ArcGIS Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="35b0f-123">Adding ArcGIS Online from the gallery</span></span>
<span data-ttu-id="35b0f-124">Při konfiguraci integrace služby ArcGIS Online do služby Azure AD, musíte přidat ArcGIS Online z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="35b0f-124">To configure the integration of ArcGIS Online into Azure AD, you need to add ArcGIS Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="35b0f-125">**Pokud chcete přidat ArcGIS Online z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="35b0f-125">**To add ArcGIS Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="35b0f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="35b0f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="35b0f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="35b0f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="35b0f-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno pro přidání nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="35b0f-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="35b0f-133">Do vyhledávacího pole zadejte **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-133">In the search box, type **ArcGIS Online**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. <span data-ttu-id="35b0f-135">Na panelu výsledků vyberte **ArcGIS Online**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="35b0f-135">In the results panel, select **ArcGIS Online**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="35b0f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="35b0f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="35b0f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ArcGIS Online na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="35b0f-138">In this section, you configure and test Azure AD single sign-on with ArcGIS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="35b0f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ArcGIS Online je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="35b0f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ArcGIS Online is to a user in Azure AD.</span></span> <span data-ttu-id="35b0f-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="35b0f-140">In other words, a link relationship between an Azure AD user and the related user in ArcGIS Online needs to be established.</span></span>

<span data-ttu-id="35b0f-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="35b0f-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ArcGIS Online.</span></span>

<span data-ttu-id="35b0f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ArcGIS Online, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="35b0f-142">To configure and test Azure AD single sign-on with ArcGIS Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="35b0f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="35b0f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="35b0f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="35b0f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="35b0f-145">**[Vytvoření ArcGIS Online testovacího uživatele](#creating-an-arcgis-online-test-user)**  – Pokud chcete mít protějšek Britta Simon v ArcGIS Online propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="35b0f-145">**[Creating an ArcGIS Online test user](#creating-an-arcgis-online-test-user)** - to have a counterpart of Britta Simon in ArcGIS Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="35b0f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="35b0f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="35b0f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="35b0f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="35b0f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="35b0f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="35b0f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="35b0f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ArcGIS Online application.</span></span>

<span data-ttu-id="35b0f-150">**Ke konfiguraci Azure AD jednotné přihlašování s ArcGIS Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="35b0f-150">**To configure Azure AD single sign-on with ArcGIS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="35b0f-151">Na portálu Azure na **ArcGIS Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-151">In the Azure portal, on the **ArcGIS Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="35b0f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="35b0f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. <span data-ttu-id="35b0f-155">Na **ArcGIS Online domény a adresy URL** část, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="35b0f-155">On the **ArcGIS Online Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    <span data-ttu-id="35b0f-157">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<company>.maps.arcgis.com`</span><span class="sxs-lookup"><span data-stu-id="35b0f-157">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<company>.maps.arcgis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="35b0f-158">Tato hodnota není reálné.</span><span class="sxs-lookup"><span data-stu-id="35b0f-158">This value is not the real.</span></span> <span data-ttu-id="35b0f-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="35b0f-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="35b0f-160">Obraťte se na [tým podpory Online klienta ArcGIS](http://support.esri.com/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="35b0f-160">Contact [ArcGIS Online Client support team](http://support.esri.com/) to get this value.</span></span> 

4. <span data-ttu-id="35b0f-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="35b0f-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. <span data-ttu-id="35b0f-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="35b0f-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="35b0f-165">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti ArcGIS.</span><span class="sxs-lookup"><span data-stu-id="35b0f-165">In a different web browser window, log into your ArcGIS company site as an administrator.</span></span>

7. <span data-ttu-id="35b0f-166">Klikněte na tlačítko **upravovat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-166">Click **EDIT SETTINGS**.</span></span>

    <span data-ttu-id="35b0f-167">![Upravit nastavení](./media/active-directory-saas-arcgis-tutorial/ic784742.png "upravit nastavení")</span><span class="sxs-lookup"><span data-stu-id="35b0f-167">![Edit Settings](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Edit Settings")</span></span>

8. <span data-ttu-id="35b0f-168">Klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-168">Click **Security**.</span></span>

    <span data-ttu-id="35b0f-169">![Zabezpečení](./media/active-directory-saas-arcgis-tutorial/ic784743.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="35b0f-169">![Security](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Security")</span></span>

9. <span data-ttu-id="35b0f-170">V části **přihlášení Enterprise**, klikněte na tlačítko **nastavit zprostředkovatele IDENTITY**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-170">Under **Enterprise Logins**, click **SET IDENTITY PROVIDER**.</span></span>

    <span data-ttu-id="35b0f-171">![Přihlášení Enterprise](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise přihlášení")</span><span class="sxs-lookup"><span data-stu-id="35b0f-171">![Enterprise Logins](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise Logins")</span></span>

10. <span data-ttu-id="35b0f-172">Na **nastavit zprostředkovatele Identity** konfigurace proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="35b0f-172">On the **Set Identity Provider** configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="35b0f-173">![Nastavení zprostředkovatele Identity](./media/active-directory-saas-arcgis-tutorial/ic784745.png "nastavit zprostředkovatele Identity")</span><span class="sxs-lookup"><span data-stu-id="35b0f-173">![Set Identity Provider](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Set Identity Provider")</span></span>
   
    <span data-ttu-id="35b0f-174">a.</span><span class="sxs-lookup"><span data-stu-id="35b0f-174">a.</span></span> <span data-ttu-id="35b0f-175">V **název** textovému poli, zadejte název vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="35b0f-175">In the **Name** textbox, type your organization’s name.</span></span>

    <span data-ttu-id="35b0f-176">b.</span><span class="sxs-lookup"><span data-stu-id="35b0f-176">b.</span></span> <span data-ttu-id="35b0f-177">Pro **Metadata pro zprostředkovatele Identity Enterprise budou předávána prostřednictvím**, vyberte **A soubor**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-177">For **Metadata for the Enterprise Identity Provider will be supplied using**, select **A File**.</span></span>

    <span data-ttu-id="35b0f-178">c.</span><span class="sxs-lookup"><span data-stu-id="35b0f-178">c.</span></span> <span data-ttu-id="35b0f-179">Chcete-li nahrát soubor stažený metadata, klikněte na tlačítko **zvolte soubor**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-179">To upload your downloaded metadata file, click **Choose file**.</span></span>

    <span data-ttu-id="35b0f-180">d.</span><span class="sxs-lookup"><span data-stu-id="35b0f-180">d.</span></span> <span data-ttu-id="35b0f-181">Klikněte na tlačítko **zprostředkovatele IDENTITY sadu**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-181">Click **SET IDENTITY PROVIDER**.</span></span>

> [!TIP]
> <span data-ttu-id="35b0f-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="35b0f-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="35b0f-183">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="35b0f-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="35b0f-184">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="35b0f-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="35b0f-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="35b0f-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="35b0f-186">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="35b0f-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="35b0f-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="35b0f-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="35b0f-189">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="35b0f-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="35b0f-191">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="35b0f-191">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="35b0f-193">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35b0f-193">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="35b0f-195">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="35b0f-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="35b0f-197">a.</span><span class="sxs-lookup"><span data-stu-id="35b0f-197">a.</span></span> <span data-ttu-id="35b0f-198">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="35b0f-199">b.</span><span class="sxs-lookup"><span data-stu-id="35b0f-199">b.</span></span> <span data-ttu-id="35b0f-200">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="35b0f-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="35b0f-201">c.</span><span class="sxs-lookup"><span data-stu-id="35b0f-201">c.</span></span> <span data-ttu-id="35b0f-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="35b0f-203">d.</span><span class="sxs-lookup"><span data-stu-id="35b0f-203">d.</span></span> <span data-ttu-id="35b0f-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-204">Click **Create**.</span></span>
 
### <a name="creating-an-arcgis-online-test-user"></a><span data-ttu-id="35b0f-205">Vytvoření ArcGIS Online testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="35b0f-205">Creating an ArcGIS Online test user</span></span>

<span data-ttu-id="35b0f-206">Pokud chcete povolit uživatelům Azure AD přihlášení do ArcGIS Online, musí být zřízená do ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="35b0f-206">In order to enable Azure AD users to log into ArcGIS Online, they must be provisioned into ArcGIS Online.</span></span>  
<span data-ttu-id="35b0f-207">V případě ArcGIS Online zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="35b0f-207">In the case of ArcGIS Online, provisioning is a manual task.</span></span>

<span data-ttu-id="35b0f-208">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="35b0f-208">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="35b0f-209">Přihlaste se k vaší **ArcGIS** klienta.</span><span class="sxs-lookup"><span data-stu-id="35b0f-209">Log in to your **ArcGIS** tenant.</span></span>

2. <span data-ttu-id="35b0f-210">Klikněte na tlačítko **POZVAT členy**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-210">Click **INVITE MEMBERS**.</span></span>
   
    <span data-ttu-id="35b0f-211">![Pozvěte členy](./media/active-directory-saas-arcgis-tutorial/ic784747.png "pozvat členy")</span><span class="sxs-lookup"><span data-stu-id="35b0f-211">![Invite Members](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Invite Members")</span></span>

3. <span data-ttu-id="35b0f-212">Vyberte **přidat členy automaticky bez odeslání e-mailu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-212">Select **Add members automatically without sending an email**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="35b0f-213">![Automaticky přidat členy](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automaticky přidat členy")</span><span class="sxs-lookup"><span data-stu-id="35b0f-213">![Add Members Automatically](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Add Members Automatically")</span></span>

4. <span data-ttu-id="35b0f-214">Na **členy** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="35b0f-214">On the **Members** dialog page, perform the following steps:</span></span>
   
     <span data-ttu-id="35b0f-215">![Přidat a zkontrolovat](./media/active-directory-saas-arcgis-tutorial/ic784749.png "přidat a zkontrolujte")</span><span class="sxs-lookup"><span data-stu-id="35b0f-215">![Add and review](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Add and review")</span></span>
    
     <span data-ttu-id="35b0f-216">a.</span><span class="sxs-lookup"><span data-stu-id="35b0f-216">a.</span></span> <span data-ttu-id="35b0f-217">Zadejte **e-mailu**, **křestní jméno**, a **příjmení** platného účtu AAD chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="35b0f-217">Enter the **Email**, **First Name**, and **Last Name** of a valid AAD account you want to provision.</span></span>
  
     <span data-ttu-id="35b0f-218">b.</span><span class="sxs-lookup"><span data-stu-id="35b0f-218">b.</span></span> <span data-ttu-id="35b0f-219">Klikněte na tlačítko **přidat a ZKONTROLUJTE**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-219">Click **ADD AND REVIEW**.</span></span>
5. <span data-ttu-id="35b0f-220">Zkontrolujte data, která jste zadali a pak klikněte na tlačítko **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-220">Review the data you have entered, and then click **ADD MEMBERS**.</span></span>
   
    <span data-ttu-id="35b0f-221">![Přidat člena](./media/active-directory-saas-arcgis-tutorial/ic784750.png "přidat člena")</span><span class="sxs-lookup"><span data-stu-id="35b0f-221">![Add member](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Add member")</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="35b0f-222">Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="35b0f-222">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="35b0f-223">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="35b0f-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="35b0f-224">V této části povolíte Britta Simon tak, že udělíte přístup k ArcGIS Online používat Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="35b0f-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ArcGIS Online.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="35b0f-226">**Pokud chcete přiřadit Britta Simon ArcGIS Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="35b0f-226">**To assign Britta Simon to ArcGIS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="35b0f-227">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="35b0f-229">V seznamu aplikací vyberte **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-229">In the applications list, select **ArcGIS Online**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. <span data-ttu-id="35b0f-231">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="35b0f-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="35b0f-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="35b0f-233">Click **Add** button.</span></span> <span data-ttu-id="35b0f-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35b0f-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="35b0f-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="35b0f-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="35b0f-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35b0f-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="35b0f-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="35b0f-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="35b0f-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="35b0f-239">Testing single sign-on</span></span>

<span data-ttu-id="35b0f-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="35b0f-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="35b0f-241">Když kliknete na dlaždici ArcGIS Online na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="35b0f-241">When you click the ArcGIS Online tile in the Access Panel, you should get automatically signed-on to your ArcGIS Online application.</span></span>
<span data-ttu-id="35b0f-242">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="35b0f-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35b0f-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="35b0f-243">Additional resources</span></span>

* [<span data-ttu-id="35b0f-244">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="35b0f-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="35b0f-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="35b0f-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

