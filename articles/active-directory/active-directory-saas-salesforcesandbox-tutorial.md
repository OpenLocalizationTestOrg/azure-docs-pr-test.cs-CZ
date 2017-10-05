---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a izolovaného prostoru služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 90e08b9cf2feb93de4877bec9734352949896dca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="796e6-103">Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="796e6-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="796e6-104">V tomto kurzu zjistěte, jak integrovat Salesforce izolovaného prostoru se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="796e6-104">In this tutorial, you learn how to integrate Salesforce Sandbox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="796e6-105">Integrace služby Salesforce izolovaného prostoru s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="796e6-105">Integrating Salesforce Sandbox with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="796e6-106">Můžete řídit ve službě Azure AD, který má přístup k izolovanému prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="796e6-106">You can control in Azure AD who has access to Salesforce Sandbox</span></span>
- <span data-ttu-id="796e6-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Salesforce izolovaného prostoru (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="796e6-107">You can enable your users to automatically get signed-on to Salesforce Sandbox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="796e6-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="796e6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="796e6-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="796e6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="796e6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="796e6-110">Prerequisites</span></span>

<span data-ttu-id="796e6-111">Konfigurace integrace Azure AD s Salesforce izolovaného prostoru, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="796e6-111">To configure Azure AD integration with Salesforce Sandbox, you need the following items:</span></span>

- <span data-ttu-id="796e6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="796e6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="796e6-113">Izolovaného prostoru Salesforce jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="796e6-113">A Salesforce Sandbox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="796e6-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="796e6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="796e6-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="796e6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="796e6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="796e6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="796e6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="796e6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="796e6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="796e6-118">Scenario description</span></span>
<span data-ttu-id="796e6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="796e6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="796e6-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="796e6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="796e6-121">Přidání izolovaného prostoru Salesforce z Galerie</span><span class="sxs-lookup"><span data-stu-id="796e6-121">Adding Salesforce Sandbox from the gallery</span></span>
2. <span data-ttu-id="796e6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="796e6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-sandbox-from-the-gallery"></a><span data-ttu-id="796e6-123">Přidání izolovaného prostoru Salesforce z Galerie</span><span class="sxs-lookup"><span data-stu-id="796e6-123">Adding Salesforce Sandbox from the gallery</span></span>
<span data-ttu-id="796e6-124">Při konfiguraci integrace služby Salesforce izolovaného prostoru do služby Azure AD potřebujete přidat izolovaného prostoru Salesforce z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="796e6-124">To configure the integration of Salesforce Sandbox into Azure AD, you need to add Salesforce Sandbox from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="796e6-125">**Pokud chcete přidat izolovaného prostoru Salesforce z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="796e6-125">**To add Salesforce Sandbox from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="796e6-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="796e6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="796e6-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="796e6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="796e6-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="796e6-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="796e6-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="796e6-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="796e6-133">Do vyhledávacího pole zadejte **izolovaného prostoru Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="796e6-133">In the search box, type **Salesforce Sandbox**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. <span data-ttu-id="796e6-135">Na panelu výsledků vyberte **izolovaného prostoru Salesforce**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="796e6-135">In the results panel, select **Salesforce Sandbox**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="796e6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="796e6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="796e6-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s izolovaného prostoru Salesforce podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="796e6-138">In this section, you configure and test Azure AD single sign-on with Salesforce Sandbox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="796e6-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Salesforce izolovaného prostoru je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="796e6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Salesforce Sandbox is to a user in Azure AD.</span></span> <span data-ttu-id="796e6-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v izolovaném prostoru Salesforce musí navázat.</span><span class="sxs-lookup"><span data-stu-id="796e6-140">In other words, a link relationship between an Azure AD user and the related user in Salesforce Sandbox needs to be established.</span></span>

<span data-ttu-id="796e6-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v izolovaném prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="796e6-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Salesforce Sandbox.</span></span>

<span data-ttu-id="796e6-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="796e6-142">To configure and test Azure AD single sign-on with Salesforce Sandbox, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="796e6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="796e6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="796e6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="796e6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="796e6-145">**[Vytvoření zkušebního uživatele izolovaného prostoru Salesforce](#creating-a-salesforce-sandbox-test-user)**  – Pokud chcete mít protějšek Britta Simon v Salesforce izolovaný prostor, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="796e6-145">**[Creating a Salesforce Sandbox test user](#creating-a-salesforce-sandbox-test-user)** - to have a counterpart of Britta Simon in Salesforce Sandbox that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="796e6-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="796e6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="796e6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="796e6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="796e6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="796e6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="796e6-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="796e6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Salesforce Sandbox application.</span></span>

<span data-ttu-id="796e6-150">**Ke konfiguraci Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="796e6-150">**To configure Azure AD single sign-on with Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="796e6-151">Na portálu Azure na **izolovaného prostoru Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="796e6-151">In the Azure portal, on the **Salesforce Sandbox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="796e6-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="796e6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. <span data-ttu-id="796e6-155">Na **Salesforce izolovaného prostoru domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="796e6-155">On the **Salesforce Sandbox Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     <span data-ttu-id="796e6-157">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="796e6-157">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<subdomain>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="796e6-158">Tato hodnota není reálné.</span><span class="sxs-lookup"><span data-stu-id="796e6-158">This value is not the real.</span></span> <span data-ttu-id="796e6-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování. Obraťte se na [tým podpory služby Salesforce izolovaného prostoru klienta](https://help.salesforce.com/support) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="796e6-159">Update this value with the actual Sign-on URL.Contact [Salesforce Sandbox Client support team](https://help.salesforce.com/support) to get this value.</span></span>


4. <span data-ttu-id="796e6-160">Pokud jste již nakonfigurovali jednotné přihlašování pro jiná instance Salesforce izolovaného prostoru v adresáři, pak je rovněž nutné nakonfigurovat **identifikátor** do mají stejnou hodnotu jako **přihlásit na adrese URL**.</span><span class="sxs-lookup"><span data-stu-id="796e6-160">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure the **Identifier** to have the same value as the **Sign on URL**.</span></span> 
    
    >[!Note]
    ><span data-ttu-id="796e6-161">**Identifikátor** pole naleznete kontrolou **zobrazit upřesňující nastavení** zaškrtávací políčko je na **konfigurace adresy URL aplikace** stránky dialogového okna</span><span class="sxs-lookup"><span data-stu-id="796e6-161">The **Identifier** field can be found by checking the **Show advanced settings** checkbox on the **Configure App URL** page of the dialog</span></span> 


5. <span data-ttu-id="796e6-162">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="796e6-162">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. <span data-ttu-id="796e6-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="796e6-164">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="796e6-166">Na **Salesforce izolovaného prostoru konfigurace** klikněte na tlačítko **konfigurace izolovaného prostoru Salesforce** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="796e6-166">On the **Salesforce Sandbox Configuration** section, click **Configure Salesforce Sandbox** to open **Configure sign-on** window.</span></span> <span data-ttu-id="796e6-167">Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="796e6-167">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    <span data-ttu-id="796e6-168">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="796e6-168">![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span></span>
8. <span data-ttu-id="796e6-169">V prohlížeči otevřete novou kartu a přihlaste se k účtu správce izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="796e6-169">Open a new tab in your browser and log in to your Salesforce Sandbox administrator account.</span></span>

9. <span data-ttu-id="796e6-170">V nabídce v horní části, klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="796e6-170">In the menu on the top, click **Setup**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. <span data-ttu-id="796e6-172">V navigačním podokně na levé straně klikněte na **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="796e6-172">In the navigation pane on the left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. <span data-ttu-id="796e6-174">V části Nastavení jednotného přihlašování, proveďte následující kroky: ![nakonfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span><span class="sxs-lookup"><span data-stu-id="796e6-174">On the Single Sign-On Settings section, perform the following steps:  ![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span></span>
     
     <span data-ttu-id="796e6-175">a.</span><span class="sxs-lookup"><span data-stu-id="796e6-175">a.</span></span>  <span data-ttu-id="796e6-176">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="796e6-176">Select **SAML Enabled**.</span></span> 

     <span data-ttu-id="796e6-177">b.</span><span class="sxs-lookup"><span data-stu-id="796e6-177">b.</span></span>  <span data-ttu-id="796e6-178">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="796e6-178">Click **New**.</span></span>

12. <span data-ttu-id="796e6-179">V části SAML jeden přihlašování nastavení proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="796e6-179">On the SAML Single Sign-On Settings section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    <span data-ttu-id="796e6-181">a.In textového pole Název zadejte název konfigurace (např: *SPSSOWAAD\_Test*).</span><span class="sxs-lookup"><span data-stu-id="796e6-181">a.In the Name textbox, type the name of the configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 

    <span data-ttu-id="796e6-182">b.</span><span class="sxs-lookup"><span data-stu-id="796e6-182">b.</span></span> <span data-ttu-id="796e6-183">Vložení **SMAL Entity ID** hodnoty do **vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="796e6-183">Paste **SMAL Entity ID** value into the **Issuer** textbox.</span></span>

    <span data-ttu-id="796e6-184">c.</span><span class="sxs-lookup"><span data-stu-id="796e6-184">c.</span></span> <span data-ttu-id="796e6-185">V **Entity Id** textovému poli, typ **https://test.salesforce.com** Pokud je první instance Salesforce izolovaného prostoru, který chcete přidat do vašeho adresáře.</span><span class="sxs-lookup"><span data-stu-id="796e6-185">In the **Entity Id** textbox, type **https://test.salesforce.com** if it is the first Salesforce Sandbox instance that you are adding to your directory.</span></span> <span data-ttu-id="796e6-186">Pokud jste již přidali instance Salesforce izolovaného prostoru, pak pro **Entity ID** zadejte **přihlašovací adresa URL**, což by mělo být v tomto formátu:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="796e6-186">If you have already added an instance of Salesforce Sandbox, then for the **Entity ID** type in the **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>  
 
    <span data-ttu-id="796e6-187">d.</span><span class="sxs-lookup"><span data-stu-id="796e6-187">d.</span></span> <span data-ttu-id="796e6-188">Klikněte na tlačítko **Procházet** nahrát stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="796e6-188">Click **Browse** to upload the downloaded certificate.</span></span>  

    <span data-ttu-id="796e6-189">e.</span><span class="sxs-lookup"><span data-stu-id="796e6-189">e.</span></span> <span data-ttu-id="796e6-190">Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje ID federace z objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="796e6-190">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span>
 
    <span data-ttu-id="796e6-191">f.</span><span class="sxs-lookup"><span data-stu-id="796e6-191">f.</span></span> <span data-ttu-id="796e6-192">Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier příkaz subjektu**.</span><span class="sxs-lookup"><span data-stu-id="796e6-192">As **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>

    <span data-ttu-id="796e6-193">g.</span><span class="sxs-lookup"><span data-stu-id="796e6-193">g.</span></span> <span data-ttu-id="796e6-194">Vložení **jeden přihlašování adresa URL služby** do **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="796e6-194">Paste **Single Sign-On Service URL** into the **Identity Provider Login URL** textbox.</span></span> 

    <span data-ttu-id="796e6-195">h.</span><span class="sxs-lookup"><span data-stu-id="796e6-195">h.</span></span> <span data-ttu-id="796e6-196">SFDC nepodporuje SAML odhlášení.</span><span class="sxs-lookup"><span data-stu-id="796e6-196">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="796e6-197">Jako alternativní řešení, vložte 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' ji do **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="796e6-197">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="796e6-198">i.</span><span class="sxs-lookup"><span data-stu-id="796e6-198">i.</span></span> <span data-ttu-id="796e6-199">Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="796e6-199">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 

    <span data-ttu-id="796e6-200">j.</span><span class="sxs-lookup"><span data-stu-id="796e6-200">j.</span></span> <span data-ttu-id="796e6-201">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="796e6-201">Click **Save**.</span></span>

### <a name="enable-your-domain"></a><span data-ttu-id="796e6-202">Povolit doménu</span><span class="sxs-lookup"><span data-stu-id="796e6-202">Enable your domain</span></span>
<span data-ttu-id="796e6-203">V této části se předpokládá, že jste již vytvořili domény.</span><span class="sxs-lookup"><span data-stu-id="796e6-203">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="796e6-204">Další informace najdete v tématu [definování váš název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="796e6-204">For more information, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="796e6-205">**Pokud chcete povolit doménu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="796e6-205">**To enable your domain, perform the following steps:**</span></span>

1. <span data-ttu-id="796e6-206">V levém navigačním podokně klikněte na tlačítko **Správa domén**a pak klikněte na tlačítko **Moje domény.**</span><span class="sxs-lookup"><span data-stu-id="796e6-206">In the left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   ><span data-ttu-id="796e6-208">Přesvědčte se, že doménu je správně nakonfigurováno.</span><span class="sxs-lookup"><span data-stu-id="796e6-208">Please make sure that your domain has been configured correctly.</span></span> 

2. <span data-ttu-id="796e6-209">V **nastavení přihlašovací stránky** klikněte na tlačítko **upravit**, pak jako **ověřovací služby**, vyberte název SAML jeden přihlašování nastavení z předchozí části a nakonec klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="796e6-209">In the **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select the name of the SAML Single Sign-On Setting from the previous section, and finally click **Save**.</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

<span data-ttu-id="796e6-211">Jakmile máte doménu nakonfigurovat, uživatelé měli používat adresa URL domény k přihlášení k izolovanému prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="796e6-211">As soon as you have a domain configured, your users should use the domain URL to login to the Salesforce sandbox.</span></span>  

<span data-ttu-id="796e6-212">Chcete-li získat hodnotu adresy URL, klikněte na tlačítko jednotného přihlašování k profilu, který jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="796e6-212">To get the value of the URL, click the SSO profile you have created in the previous section.</span></span>    

> [!TIP]
> <span data-ttu-id="796e6-213">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="796e6-213">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="796e6-214">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="796e6-214">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="796e6-215">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="796e6-215">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="796e6-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="796e6-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="796e6-217">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="796e6-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="796e6-219">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="796e6-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="796e6-220">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="796e6-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="796e6-222">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="796e6-222">to display the list of users go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="796e6-224">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="796e6-224">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="796e6-226">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="796e6-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="796e6-228">a.</span><span class="sxs-lookup"><span data-stu-id="796e6-228">a.</span></span> <span data-ttu-id="796e6-229">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="796e6-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="796e6-230">b.</span><span class="sxs-lookup"><span data-stu-id="796e6-230">b.</span></span> <span data-ttu-id="796e6-231">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="796e6-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="796e6-232">c.</span><span class="sxs-lookup"><span data-stu-id="796e6-232">c.</span></span> <span data-ttu-id="796e6-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="796e6-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="796e6-234">d.</span><span class="sxs-lookup"><span data-stu-id="796e6-234">d.</span></span> <span data-ttu-id="796e6-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="796e6-235">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-sandbox-test-user"></a><span data-ttu-id="796e6-236">Vytvoření zkušebního uživatele izolovaného prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="796e6-236">Creating a Salesforce Sandbox test user</span></span>

<span data-ttu-id="796e6-237">V této části se uživatel volá Britta Simon vytvoří v izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="796e6-237">In this section, a user called Britta Simon is created in Salesforce Sandbox.</span></span> <span data-ttu-id="796e6-238">Izolovaný prostor Salesforce podporuje zřizování za běhu, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="796e6-238">Salesforce Sandbox supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="796e6-239">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="796e6-239">There is no action item for you in this section.</span></span> <span data-ttu-id="796e6-240">Pokud uživatel v izolovaném prostoru Salesforce ještě neexistuje, vytvoří se nový při pokusu o přístup k izolovanému prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="796e6-240">If a user doesn't already exist in Salesforce Sandbox, a new one is created when you attempt to access Salesforce Sandbox.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="796e6-241">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="796e6-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="796e6-242">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k izolovanému prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="796e6-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Salesforce Sandbox.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="796e6-244">**Pokud chcete přiřadit Britta Simon Salesforce izolovaného prostoru, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="796e6-244">**To assign Britta Simon to Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="796e6-245">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="796e6-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="796e6-247">V seznamu aplikací vyberte **izolovaného prostoru Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="796e6-247">In the applications list, select **Salesforce Sandbox**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. <span data-ttu-id="796e6-249">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="796e6-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="796e6-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="796e6-251">Click **Add** button.</span></span> <span data-ttu-id="796e6-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="796e6-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="796e6-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="796e6-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="796e6-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="796e6-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="796e6-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="796e6-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="796e6-257">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="796e6-257">Testing single sign-on</span></span>

<span data-ttu-id="796e6-258">Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="796e6-258">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="796e6-259">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="796e6-259">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="796e6-260">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="796e6-260">Additional resources</span></span>

* [<span data-ttu-id="796e6-261">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="796e6-261">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="796e6-262">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="796e6-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="796e6-263">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="796e6-263">Configure User Provisioning</span></span>](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

