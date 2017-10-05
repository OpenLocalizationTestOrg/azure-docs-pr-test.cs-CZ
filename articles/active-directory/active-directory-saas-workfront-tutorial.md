---
title: 'Kurz: Azure Active Directory integrace s Workfront | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Workfront."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: f7ba8d4895474de0da0e04da5f31959963ae65ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workfront"></a><span data-ttu-id="e75bd-103">Kurz: Azure Active Directory integrace s Workfront</span><span class="sxs-lookup"><span data-stu-id="e75bd-103">Tutorial: Azure Active Directory integration with Workfront</span></span>

<span data-ttu-id="e75bd-104">V tomto kurzu zjistěte, jak integrovat Workfront s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e75bd-104">In this tutorial, you learn how to integrate Workfront with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e75bd-105">Integrace Workfront s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e75bd-105">Integrating Workfront with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e75bd-106">Můžete řídit ve službě Azure AD, který má přístup k Workfront</span><span class="sxs-lookup"><span data-stu-id="e75bd-106">You can control in Azure AD who has access to Workfront</span></span>
- <span data-ttu-id="e75bd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Workfront (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e75bd-107">You can enable your users to automatically get signed-on to Workfront (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e75bd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e75bd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e75bd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e75bd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e75bd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e75bd-110">Prerequisites</span></span>

<span data-ttu-id="e75bd-111">Konfigurace integrace Azure AD s Workfront, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e75bd-111">To configure Azure AD integration with Workfront, you need the following items:</span></span>

- <span data-ttu-id="e75bd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e75bd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e75bd-113">Workfront jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e75bd-113">A Workfront single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e75bd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e75bd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e75bd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e75bd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e75bd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e75bd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e75bd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e75bd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e75bd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e75bd-118">Scenario description</span></span>
<span data-ttu-id="e75bd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e75bd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e75bd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e75bd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e75bd-121">Přidání Workfront z Galerie</span><span class="sxs-lookup"><span data-stu-id="e75bd-121">Adding Workfront from the gallery</span></span>
2. <span data-ttu-id="e75bd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e75bd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workfront-from-the-gallery"></a><span data-ttu-id="e75bd-123">Přidání Workfront z Galerie</span><span class="sxs-lookup"><span data-stu-id="e75bd-123">Adding Workfront from the gallery</span></span>
<span data-ttu-id="e75bd-124">Při konfiguraci integrace Workfront do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Workfront z galerie.</span><span class="sxs-lookup"><span data-stu-id="e75bd-124">To configure the integration of Workfront into Azure AD, you need to add Workfront from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e75bd-125">**Pokud chcete přidat Workfront z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e75bd-125">**To add Workfront from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e75bd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e75bd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e75bd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e75bd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e75bd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e75bd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e75bd-133">Do vyhledávacího pole zadejte **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-133">In the search box, type **Workfront**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_search.png)

5. <span data-ttu-id="e75bd-135">Na panelu výsledků vyberte **Workfront**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e75bd-135">In the results panel, select **Workfront**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e75bd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e75bd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e75bd-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workfront podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e75bd-138">In this section, you configure and test Azure AD single sign-on with Workfront based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e75bd-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Workfront je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e75bd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workfront is to a user in Azure AD.</span></span> <span data-ttu-id="e75bd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Workfront musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e75bd-140">In other words, a link relationship between an Azure AD user and the related user in Workfront needs to be established.</span></span>

<span data-ttu-id="e75bd-141">V Workfront, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e75bd-141">In Workfront, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e75bd-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workfront, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e75bd-142">To configure and test Azure AD single sign-on with Workfront, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e75bd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e75bd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e75bd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e75bd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e75bd-145">**[Vytvoření zkušebního uživatele Workfront](#creating-a-workfront-test-user)**  – Pokud chcete mít protějšek Britta Simon v Workfront propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e75bd-145">**[Creating a Workfront test user](#creating-a-workfront-test-user)** - to have a counterpart of Britta Simon in Workfront that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e75bd-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e75bd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e75bd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e75bd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e75bd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e75bd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e75bd-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Workfront.</span><span class="sxs-lookup"><span data-stu-id="e75bd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workfront application.</span></span>

<span data-ttu-id="e75bd-150">**Ke konfiguraci Azure AD jednotné přihlašování s Workfront, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e75bd-150">**To configure Azure AD single sign-on with Workfront, perform the following steps:**</span></span>

1. <span data-ttu-id="e75bd-151">Na portálu Azure na **Workfront** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-151">In the Azure portal, on the **Workfront** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e75bd-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e75bd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_samlbase.png)

3. <span data-ttu-id="e75bd-155">Na **Workfront domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e75bd-155">On the **Workfront Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_url.png)

    <span data-ttu-id="e75bd-157">a.</span><span class="sxs-lookup"><span data-stu-id="e75bd-157">a.</span></span> <span data-ttu-id="e75bd-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.attask-ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="e75bd-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.attask-ondemand.com`</span></span>

    <span data-ttu-id="e75bd-159">b.</span><span class="sxs-lookup"><span data-stu-id="e75bd-159">b.</span></span> <span data-ttu-id="e75bd-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.attasksandbox.com/SAML2`</span><span class="sxs-lookup"><span data-stu-id="e75bd-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.attasksandbox.com/SAML2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e75bd-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e75bd-161">These values are not real.</span></span> <span data-ttu-id="e75bd-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e75bd-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e75bd-163">Obraťte se na [tým podpory Workfront klienta](https://www.workfront.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e75bd-163">Contact [Workfront Client support team](https://www.workfront.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="e75bd-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="e75bd-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_certificate.png) 

5. <span data-ttu-id="e75bd-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e75bd-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e75bd-168">Na **Workfront konfigurace** klikněte na tlačítko **konfigurace Workfront** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e75bd-168">On the **Workfront Configuration** section, click **Configure Workfront** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e75bd-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e75bd-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_configure.png) 

7. <span data-ttu-id="e75bd-171">Přihlašování k webu společnosti Workfront jako správce.</span><span class="sxs-lookup"><span data-stu-id="e75bd-171">Sign-on to your Workfront company site as administrator.</span></span>

8. <span data-ttu-id="e75bd-172">Přejděte na **jednotné přihlašování v konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-172">Go to **Single Sign On Configuration**.</span></span>

9. <span data-ttu-id="e75bd-173">Na **jednotné přihlašování** dialogové okno, proveďte následující kroky</span><span class="sxs-lookup"><span data-stu-id="e75bd-173">On the **Single Sign-On** dialog, perform the following steps</span></span>
    
    ![Konfigurovat jednotné přihlašování][23]
   
    <span data-ttu-id="e75bd-175">a.</span><span class="sxs-lookup"><span data-stu-id="e75bd-175">a.</span></span> <span data-ttu-id="e75bd-176">Jako **typ**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-176">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="e75bd-177">b.</span><span class="sxs-lookup"><span data-stu-id="e75bd-177">b.</span></span> <span data-ttu-id="e75bd-178">Vyberte **služby ID zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-178">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="e75bd-179">c.</span><span class="sxs-lookup"><span data-stu-id="e75bd-179">c.</span></span> <span data-ttu-id="e75bd-180">Vložení **SAML jeden přihlašování adresa URL služby** do **adresu URL pro přihlášení portálu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e75bd-180">Paste the **SAML Single Sign-On Service URL** into the **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="e75bd-181">d.</span><span class="sxs-lookup"><span data-stu-id="e75bd-181">d.</span></span> <span data-ttu-id="e75bd-182">Vložení **jednu adresu URL služby Sign-Out** do **Sign-Out URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e75bd-182">Paste **Single Sign-Out Service URL** into the **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="e75bd-183">e.</span><span class="sxs-lookup"><span data-stu-id="e75bd-183">e.</span></span> <span data-ttu-id="e75bd-184">Vložení **heslo změnit adresu URL** do **heslo změnit adresu URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e75bd-184">Paste **Change Password URL** into the **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="e75bd-185">f.</span><span class="sxs-lookup"><span data-stu-id="e75bd-185">f.</span></span> <span data-ttu-id="e75bd-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e75bd-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e75bd-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e75bd-188">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e75bd-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e75bd-189">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e75bd-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e75bd-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e75bd-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="e75bd-191">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e75bd-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e75bd-193">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e75bd-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e75bd-194">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e75bd-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e75bd-196">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e75bd-198">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e75bd-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e75bd-200">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e75bd-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e75bd-202">a.</span><span class="sxs-lookup"><span data-stu-id="e75bd-202">a.</span></span> <span data-ttu-id="e75bd-203">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e75bd-204">b.</span><span class="sxs-lookup"><span data-stu-id="e75bd-204">b.</span></span> <span data-ttu-id="e75bd-205">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e75bd-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e75bd-206">c.</span><span class="sxs-lookup"><span data-stu-id="e75bd-206">c.</span></span> <span data-ttu-id="e75bd-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e75bd-208">d.</span><span class="sxs-lookup"><span data-stu-id="e75bd-208">d.</span></span> <span data-ttu-id="e75bd-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-209">Click **Create**.</span></span>
 
### <a name="creating-a-workfront-test-user"></a><span data-ttu-id="e75bd-210">Vytvoření zkušebního uživatele Workfront</span><span class="sxs-lookup"><span data-stu-id="e75bd-210">Creating a Workfront test user</span></span>

<span data-ttu-id="e75bd-211">Cílem této části je vytvoření uživatele v Workfront nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e75bd-211">The objective of this section is to create a user called Britta Simon in Workfront.</span></span>

<span data-ttu-id="e75bd-212">**Vytvoření uživatele v Workfront nazývá Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e75bd-212">**To create a user called Britta Simon in Workfront, perform the following steps:**</span></span>

1. <span data-ttu-id="e75bd-213">Přihlásit se k serveru vaší společnosti Workfront jako správce.</span><span class="sxs-lookup"><span data-stu-id="e75bd-213">Sign on to your Workfront company site as administrator.</span></span>
2. <span data-ttu-id="e75bd-214">V nabídce v horní části, klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-214">In the menu on the top, click **People**.</span></span>
3. <span data-ttu-id="e75bd-215">Klikněte na tlačítko **nové osobě**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-215">Click **New Person**.</span></span> 
4. <span data-ttu-id="e75bd-216">V dialogovém okně nové osobě proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e75bd-216">On the New Person dialog, perform the following steps:</span></span>
   
    ![Vytvořit uživatele s Workfront testu][21] 
   
    <span data-ttu-id="e75bd-218">a.</span><span class="sxs-lookup"><span data-stu-id="e75bd-218">a.</span></span> <span data-ttu-id="e75bd-219">V **křestní jméno** textovému poli, zadejte "Britta."</span><span class="sxs-lookup"><span data-stu-id="e75bd-219">In the **First Name** textbox, type "Britta."</span></span>
   
    <span data-ttu-id="e75bd-220">b.</span><span class="sxs-lookup"><span data-stu-id="e75bd-220">b.</span></span> <span data-ttu-id="e75bd-221">V **příjmení** textovému poli, zadejte "Simon."</span><span class="sxs-lookup"><span data-stu-id="e75bd-221">In the **Last Name** textbox, type "Simon."</span></span>
   
    <span data-ttu-id="e75bd-222">c.</span><span class="sxs-lookup"><span data-stu-id="e75bd-222">c.</span></span> <span data-ttu-id="e75bd-223">V **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta Simon v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e75bd-223">In the **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="e75bd-224">d.</span><span class="sxs-lookup"><span data-stu-id="e75bd-224">d.</span></span> <span data-ttu-id="e75bd-225">Klikněte na tlačítko **přidat osobu**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-225">Click **Add Person**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e75bd-226">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e75bd-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e75bd-227">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Workfront.</span><span class="sxs-lookup"><span data-stu-id="e75bd-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workfront.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e75bd-229">**Pokud chcete přiřadit Britta Simon Workfront, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e75bd-229">**To assign Britta Simon to Workfront, perform the following steps:**</span></span>

1. <span data-ttu-id="e75bd-230">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e75bd-232">V seznamu aplikací vyberte **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-232">In the applications list, select **Workfront**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_app.png) 

3. <span data-ttu-id="e75bd-234">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e75bd-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e75bd-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e75bd-236">Click **Add** button.</span></span> <span data-ttu-id="e75bd-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e75bd-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e75bd-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e75bd-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e75bd-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e75bd-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e75bd-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e75bd-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e75bd-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e75bd-242">Testing single sign-on</span></span>

<span data-ttu-id="e75bd-243">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e75bd-243">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e75bd-244">Když kliknete na dlaždici Workfront na přístupovém panelu, měli byste obdržet přihlašovací stránku Workfront aplikace.</span><span class="sxs-lookup"><span data-stu-id="e75bd-244">When you click the Workfront tile in the Access Panel, you should get login page of Workfront application.</span></span>
<span data-ttu-id="e75bd-245">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e75bd-245">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e75bd-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e75bd-246">Additional resources</span></span>

* [<span data-ttu-id="e75bd-247">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e75bd-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e75bd-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e75bd-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_04.png
[21]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_08.png
[23]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_06.png
[100]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_203.png

