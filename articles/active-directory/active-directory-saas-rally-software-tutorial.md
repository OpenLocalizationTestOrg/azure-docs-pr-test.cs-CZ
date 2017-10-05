---
title: 'Kurz: Azure Active Directory integrace s technologie Rally softwarem | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a technologie Rally softwaru."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: 6481c9ef0ca71419ccfa6f7956f4702985743df3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a><span data-ttu-id="dcf70-103">Kurz: Azure Active Directory integrace s technologie Rally softwaru</span><span class="sxs-lookup"><span data-stu-id="dcf70-103">Tutorial: Azure Active Directory integration with Rally Software</span></span>

<span data-ttu-id="dcf70-104">V tomto kurzu zjistěte, jak integrovat technologie Rally softwaru s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dcf70-104">In this tutorial, you learn how to integrate Rally Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dcf70-105">Integrace technologie Rally softwaru s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="dcf70-105">Integrating Rally Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dcf70-106">Můžete ovládat ve službě Azure AD, který má přístup k technologie Rally softwaru.</span><span class="sxs-lookup"><span data-stu-id="dcf70-106">You can control in Azure AD who has access to Rally Software.</span></span>
- <span data-ttu-id="dcf70-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k technologie Rally softwaru (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dcf70-107">You can enable your users to automatically get signed-on to Rally Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="dcf70-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dcf70-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="dcf70-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dcf70-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dcf70-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dcf70-110">Prerequisites</span></span>

<span data-ttu-id="dcf70-111">Konfigurace integrace Azure AD s technologie Rally softwaru, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="dcf70-111">To configure Azure AD integration with Rally Software, you need the following items:</span></span>

- <span data-ttu-id="dcf70-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="dcf70-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dcf70-113">Technologie Rally softwaru jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="dcf70-113">A Rally Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dcf70-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="dcf70-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dcf70-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="dcf70-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dcf70-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="dcf70-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dcf70-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dcf70-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dcf70-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="dcf70-118">Scenario description</span></span>
<span data-ttu-id="dcf70-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="dcf70-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dcf70-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="dcf70-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dcf70-121">Přidání technologie Rally softwaru z Galerie</span><span class="sxs-lookup"><span data-stu-id="dcf70-121">Adding Rally Software from the gallery</span></span>
2. <span data-ttu-id="dcf70-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dcf70-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rally-software-from-the-gallery"></a><span data-ttu-id="dcf70-123">Přidání technologie Rally softwaru z Galerie</span><span class="sxs-lookup"><span data-stu-id="dcf70-123">Adding Rally Software from the gallery</span></span>
<span data-ttu-id="dcf70-124">Při konfiguraci integrace technologie Rally softwaru do služby Azure AD, musíte přidat technologie Rally Software z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="dcf70-124">To configure the integration of Rally Software into Azure AD, you need to add Rally Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dcf70-125">**Pokud chcete přidat technologie Rally softwaru z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dcf70-125">**To add Rally Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dcf70-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dcf70-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="dcf70-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dcf70-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="dcf70-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dcf70-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="dcf70-133">Do vyhledávacího pole zadejte **technologie Rally softwaru**, vyberte **technologie Rally softwaru** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dcf70-133">In the search box, type **Rally Software**, select **Rally Software** from result panel then click **Add** button to add the application.</span></span>

    ![V seznamu výsledků technologie Rally softwaru](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="dcf70-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dcf70-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="dcf70-136">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí technologie Rally Software založený na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="dcf70-136">In this section, you configure and test Azure AD single sign-on with Rally Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dcf70-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v technologie Rally softwaru je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dcf70-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Rally Software is to a user in Azure AD.</span></span> <span data-ttu-id="dcf70-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v technologie Rally softwaru je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="dcf70-138">In other words, a link relationship between an Azure AD user and the related user in Rally Software needs to be established.</span></span>

<span data-ttu-id="dcf70-139">V softwaru technologie Rally přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="dcf70-139">In Rally Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dcf70-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s technologie Rally softwaru, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="dcf70-140">To configure and test Azure AD single sign-on with Rally Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dcf70-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="dcf70-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dcf70-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dcf70-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dcf70-143">**[Vytvoření zkušebního uživatele technologie Rally softwaru](#create-a-rally-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v technologie Rally Software, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="dcf70-143">**[Create a Rally Software test user](#create-a-rally-software-test-user)** - to have a counterpart of Britta Simon in Rally Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dcf70-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dcf70-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dcf70-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dcf70-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="dcf70-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dcf70-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="dcf70-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci technologie Rally softwaru.</span><span class="sxs-lookup"><span data-stu-id="dcf70-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Rally Software application.</span></span>

<span data-ttu-id="dcf70-148">**Ke konfiguraci Azure AD jednotné přihlašování s technologie Rally softwarem, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dcf70-148">**To configure Azure AD single sign-on with Rally Software, perform the following steps:**</span></span>

1. <span data-ttu-id="dcf70-149">Na portálu Azure na **technologie Rally softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-149">In the Azure portal, on the **Rally Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="dcf70-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dcf70-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. <span data-ttu-id="dcf70-153">Na **technologie Rally softwaru domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dcf70-153">On the **Rally Software Domain and URLs** section, perform the following steps:</span></span>

    ![Technologie Rally softwaru adresy URL jeden přihlašování informace o doméně a](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    <span data-ttu-id="dcf70-155">a.</span><span class="sxs-lookup"><span data-stu-id="dcf70-155">a.</span></span> <span data-ttu-id="dcf70-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="dcf70-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.rally.com`</span></span>

    <span data-ttu-id="dcf70-157">b.</span><span class="sxs-lookup"><span data-stu-id="dcf70-157">b.</span></span> <span data-ttu-id="dcf70-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="dcf70-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.rally.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dcf70-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="dcf70-159">These values are not real.</span></span> <span data-ttu-id="dcf70-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="dcf70-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="dcf70-161">Obraťte se na [tým podpory technologie Rally klientský Software](https://help.rallydev.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="dcf70-161">Contact [Rally Software Client support team](https://help.rallydev.com/) to get these values.</span></span> 
 


4. <span data-ttu-id="dcf70-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="dcf70-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. <span data-ttu-id="dcf70-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcf70-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dcf70-166">Na **technologie Rally softwarové konfigurace** klikněte na tlačítko **konfigurace technologie Rally softwaru** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="dcf70-166">On the **Rally Software Configuration** section, click **Configure Rally Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dcf70-167">Kopírování **Sign-Out adresy URL a SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="dcf70-167">Copy the **Sign-Out URL, and SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Technologie Rally konfigurace softwaru](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. <span data-ttu-id="dcf70-169">Přihlaste se k vaší **technologie Rally softwaru** klienta.</span><span class="sxs-lookup"><span data-stu-id="dcf70-169">Log in to your **Rally Software** tenant.</span></span>

8. <span data-ttu-id="dcf70-170">Na panelu nástrojů v horní části klikněte na tlačítko **instalace**a potom vyberte **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-170">In the toolbar on the top, click **Setup**, and then select **Subscription**.</span></span>
   
    <span data-ttu-id="dcf70-171">![Předplatné](./media/active-directory-saas-rally-software-tutorial/ic769531.png "předplatného")</span><span class="sxs-lookup"><span data-stu-id="dcf70-171">![Subscription](./media/active-directory-saas-rally-software-tutorial/ic769531.png "Subscription")</span></span>

9. <span data-ttu-id="dcf70-172">Klikněte **akce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcf70-172">Click the **Action** button.</span></span> <span data-ttu-id="dcf70-173">Vyberte **upravit předplatné** v pravé horní části panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="dcf70-173">Select **Edit Subscription** at the top right side of the toolbar.</span></span>

10. <span data-ttu-id="dcf70-174">Na **předplatné** dialogové okno stránky, proveďte následující kroky a pak klikněte na **uložit a zavřít**:</span><span class="sxs-lookup"><span data-stu-id="dcf70-174">On the **Subscription** dialog page, perform the following steps, and then click **Save & Close**:</span></span>
   
    <span data-ttu-id="dcf70-175">![Ověřování](./media/active-directory-saas-rally-software-tutorial/ic769542.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="dcf70-175">![Authentication](./media/active-directory-saas-rally-software-tutorial/ic769542.png "Authentication")</span></span>
   
    <span data-ttu-id="dcf70-176">a.</span><span class="sxs-lookup"><span data-stu-id="dcf70-176">a.</span></span> <span data-ttu-id="dcf70-177">Vyberte **technologie Rally nebo jednotného přihlašování k ověřování** z rozevíracího seznamu ověřování.</span><span class="sxs-lookup"><span data-stu-id="dcf70-177">Select **Rally or SSO authentication** from Authentication dropdown.</span></span>

    <span data-ttu-id="dcf70-178">b.</span><span class="sxs-lookup"><span data-stu-id="dcf70-178">b.</span></span> <span data-ttu-id="dcf70-179">V **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dcf70-179">In the **Identity provider URL** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="dcf70-180">c.</span><span class="sxs-lookup"><span data-stu-id="dcf70-180">c.</span></span> <span data-ttu-id="dcf70-181">V **jednotného přihlašování k odhlášení** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dcf70-181">In the **SSO Logout** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="dcf70-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="dcf70-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dcf70-183">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="dcf70-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dcf70-184">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dcf70-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="dcf70-185">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dcf70-185">Create an Azure AD test user</span></span>

<span data-ttu-id="dcf70-186">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dcf70-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="dcf70-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dcf70-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dcf70-189">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcf70-189">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="dcf70-191">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-191">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="dcf70-193">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dcf70-193">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="dcf70-195">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dcf70-195">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    <span data-ttu-id="dcf70-197">a.</span><span class="sxs-lookup"><span data-stu-id="dcf70-197">a.</span></span> <span data-ttu-id="dcf70-198">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-198">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dcf70-199">b.</span><span class="sxs-lookup"><span data-stu-id="dcf70-199">b.</span></span> <span data-ttu-id="dcf70-200">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dcf70-200">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="dcf70-201">c.</span><span class="sxs-lookup"><span data-stu-id="dcf70-201">c.</span></span> <span data-ttu-id="dcf70-202">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="dcf70-202">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="dcf70-203">d.</span><span class="sxs-lookup"><span data-stu-id="dcf70-203">d.</span></span> <span data-ttu-id="dcf70-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-204">Click **Create**.</span></span>
 
### <a name="create-a-rally-software-test-user"></a><span data-ttu-id="dcf70-205">Vytvoření zkušebního uživatele technologie Rally softwaru</span><span class="sxs-lookup"><span data-stu-id="dcf70-205">Create a Rally Software test user</span></span>

<span data-ttu-id="dcf70-206">Azure AD uživatelé moci přihlásit musí být zřízená do technologie Rally softwaru aplikace pomocí jejich názvy uživatele Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="dcf70-206">For Azure AD users to be able to sign in, they must be provisioned to the Rally Software application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="dcf70-207">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dcf70-207">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="dcf70-208">Přihlaste se ke klientovi technologie Rally softwaru.</span><span class="sxs-lookup"><span data-stu-id="dcf70-208">Log in to your Rally Software tenant.</span></span>

2. <span data-ttu-id="dcf70-209">Přejděte na **instalace \> uživatelé**a potom klikněte na **+ přidat nový**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-209">Go to **Setup \> USERS**, and then click **+ Add New**.</span></span>
   
    <span data-ttu-id="dcf70-210">![Uživatelé](./media/active-directory-saas-rally-software-tutorial/ic781039.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="dcf70-210">![Users](./media/active-directory-saas-rally-software-tutorial/ic781039.png "Users")</span></span>

3. <span data-ttu-id="dcf70-211">Zadejte název do textového pole nového uživatele a pak klikněte na tlačítko **přidat s podrobnostmi**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-211">Type the name in the New User textbox, and then click **Add with Details**.</span></span>

4. <span data-ttu-id="dcf70-212">V **vytvořit uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dcf70-212">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="dcf70-213">![Vytvoření uživatele](./media/active-directory-saas-rally-software-tutorial/ic781040.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="dcf70-213">![Create User](./media/active-directory-saas-rally-software-tutorial/ic781040.png "Create User")</span></span>

    <span data-ttu-id="dcf70-214">a.</span><span class="sxs-lookup"><span data-stu-id="dcf70-214">a.</span></span> <span data-ttu-id="dcf70-215">V **uživatelské jméno** textovému poli, zadejte jméno uživatele jako **Brittsimon**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-215">In the **User Name** textbox, type the name of user like **Brittsimon**.</span></span>
   
    <span data-ttu-id="dcf70-216">b.</span><span class="sxs-lookup"><span data-stu-id="dcf70-216">b.</span></span> <span data-ttu-id="dcf70-217">V **e-mailovou adresu** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="dcf70-217">In **E-mail Address** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="dcf70-218">c.</span><span class="sxs-lookup"><span data-stu-id="dcf70-218">c.</span></span> <span data-ttu-id="dcf70-219">V **křestní jméno** textové pole, zadejte jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-219">In **First Name** text box, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="dcf70-220">d.</span><span class="sxs-lookup"><span data-stu-id="dcf70-220">d.</span></span> <span data-ttu-id="dcf70-221">V **příjmení** text zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-221">In **Last Name** text box, enter the last name of user like **Simon**.</span></span>

    <span data-ttu-id="dcf70-222">e.</span><span class="sxs-lookup"><span data-stu-id="dcf70-222">e.</span></span> <span data-ttu-id="dcf70-223">Klikněte na tlačítko **uložit a zavřít**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-223">Click **Save & Close**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="dcf70-224">Další nástroje pro tvorbu technologie Rally softwaru uživatelského účtu nebo rozhraní API poskytovaných technologie Rally softwaru můžete použít ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dcf70-224">You can use any other Rally Software user account creation tools or APIs provided by Rally Software to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="dcf70-225">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dcf70-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="dcf70-226">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu technologie Rally softwaru.</span><span class="sxs-lookup"><span data-stu-id="dcf70-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Rally Software.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="dcf70-228">**Pokud chcete přiřadit Britta Simon technologie Rally softwaru, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dcf70-228">**To assign Britta Simon to Rally Software, perform the following steps:**</span></span>

1. <span data-ttu-id="dcf70-229">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="dcf70-231">V seznamu aplikací vyberte **technologie Rally softwaru**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-231">In the applications list, select **Rally Software**.</span></span>

    ![V seznamu aplikací na odkaz technologie Rally softwaru](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. <span data-ttu-id="dcf70-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dcf70-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="dcf70-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcf70-235">Click **Add** button.</span></span> <span data-ttu-id="dcf70-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dcf70-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="dcf70-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dcf70-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dcf70-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dcf70-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dcf70-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dcf70-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="dcf70-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dcf70-241">Test single sign-on</span></span>

<span data-ttu-id="dcf70-242">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="dcf70-242">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dcf70-243">Když kliknete na dlaždici technologie Rally softwaru na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci technologie Rally softwaru.</span><span class="sxs-lookup"><span data-stu-id="dcf70-243">When you click the Rally Software tile in the Access Panel, you should get automatically signed-on to your Rally Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dcf70-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dcf70-244">Additional resources</span></span>

* [<span data-ttu-id="dcf70-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dcf70-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dcf70-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dcf70-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

