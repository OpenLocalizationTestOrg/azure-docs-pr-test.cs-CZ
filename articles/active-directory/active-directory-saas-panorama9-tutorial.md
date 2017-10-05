---
title: 'Kurz: Azure Active Directory integrace s Panorama9 | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Panorama9."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 934c0743464fd32398071aa3d07f7af76fdf7e3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a><span data-ttu-id="3e2da-103">Kurz: Azure Active Directory integrace s Panorama9</span><span class="sxs-lookup"><span data-stu-id="3e2da-103">Tutorial: Azure Active Directory integration with Panorama9</span></span>

<span data-ttu-id="3e2da-104">V tomto kurzu zjistěte, jak integrovat Panorama9 s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3e2da-104">In this tutorial, you learn how to integrate Panorama9 with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3e2da-105">Integrace Panorama9 s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3e2da-105">Integrating Panorama9 with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3e2da-106">Můžete řídit ve službě Azure AD, který má přístup k Panorama9</span><span class="sxs-lookup"><span data-stu-id="3e2da-106">You can control in Azure AD who has access to Panorama9</span></span>
- <span data-ttu-id="3e2da-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Panorama9 (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e2da-107">You can enable your users to automatically get signed-on to Panorama9 (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3e2da-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3e2da-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3e2da-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3e2da-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e2da-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e2da-110">Prerequisites</span></span>

<span data-ttu-id="3e2da-111">Konfigurace integrace Azure AD s Panorama9, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3e2da-111">To configure Azure AD integration with Panorama9, you need the following items:</span></span>

- <span data-ttu-id="3e2da-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e2da-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3e2da-113">Panorama9 jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3e2da-113">A Panorama9 single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3e2da-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e2da-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3e2da-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3e2da-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3e2da-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3e2da-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3e2da-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3e2da-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3e2da-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3e2da-118">Scenario description</span></span>
<span data-ttu-id="3e2da-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e2da-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3e2da-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3e2da-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3e2da-121">Přidání Panorama9 z Galerie</span><span class="sxs-lookup"><span data-stu-id="3e2da-121">Adding Panorama9 from the gallery</span></span>
2. <span data-ttu-id="3e2da-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e2da-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panorama9-from-the-gallery"></a><span data-ttu-id="3e2da-123">Přidání Panorama9 z Galerie</span><span class="sxs-lookup"><span data-stu-id="3e2da-123">Adding Panorama9 from the gallery</span></span>
<span data-ttu-id="3e2da-124">Při konfiguraci integrace Panorama9 do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Panorama9 z galerie.</span><span class="sxs-lookup"><span data-stu-id="3e2da-124">To configure the integration of Panorama9 into Azure AD, you need to add Panorama9 from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3e2da-125">**Pokud chcete přidat Panorama9 z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e2da-125">**To add Panorama9 from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3e2da-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3e2da-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3e2da-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3e2da-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3e2da-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e2da-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3e2da-133">Do vyhledávacího pole zadejte **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-133">In the search box, type **Panorama9**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_search.png)

5. <span data-ttu-id="3e2da-135">Na panelu výsledků vyberte **Panorama9**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e2da-135">In the results panel, select **Panorama9**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3e2da-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e2da-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="3e2da-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Panorama9 podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3e2da-138">In this section, you configure and test Azure AD single sign-on with Panorama9 based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3e2da-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Panorama9 je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e2da-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Panorama9 is to a user in Azure AD.</span></span> <span data-ttu-id="3e2da-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Panorama9 musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3e2da-140">In other words, a link relationship between an Azure AD user and the related user in Panorama9 needs to be established.</span></span>

<span data-ttu-id="3e2da-141">V Panorama9, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3e2da-141">In Panorama9, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3e2da-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Panorama9, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3e2da-142">To configure and test Azure AD single sign-on with Panorama9, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3e2da-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3e2da-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3e2da-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e2da-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3e2da-145">**[Vytvoření zkušebního uživatele Panorama9](#creating-a-panorama9-test-user)**  – Pokud chcete mít protějšek Britta Simon v Panorama9 propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e2da-145">**[Creating a Panorama9 test user](#creating-a-panorama9-test-user)** - to have a counterpart of Britta Simon in Panorama9 that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3e2da-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e2da-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3e2da-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3e2da-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3e2da-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e2da-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3e2da-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Panorama9.</span><span class="sxs-lookup"><span data-stu-id="3e2da-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Panorama9 application.</span></span>

<span data-ttu-id="3e2da-150">**Ke konfiguraci Azure AD jednotné přihlašování s Panorama9, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e2da-150">**To configure Azure AD single sign-on with Panorama9, perform the following steps:**</span></span>

1. <span data-ttu-id="3e2da-151">Na portálu Azure na **Panorama9** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-151">In the Azure portal, on the **Panorama9** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3e2da-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e2da-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. <span data-ttu-id="3e2da-155">Na **Panorama9 domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e2da-155">On the **Panorama9 Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_url.png)

    <span data-ttu-id="3e2da-157">a.</span><span class="sxs-lookup"><span data-stu-id="3e2da-157">a.</span></span> <span data-ttu-id="3e2da-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://dashboard.panorama9.com/saml/access/3262`</span><span class="sxs-lookup"><span data-stu-id="3e2da-158">In the **Sign-on URL** textbox, type a URL as: `https://dashboard.panorama9.com/saml/access/3262`</span></span>

    <span data-ttu-id="3e2da-159">b.</span><span class="sxs-lookup"><span data-stu-id="3e2da-159">b.</span></span> <span data-ttu-id="3e2da-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://www.panorama9.com/saml20/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="3e2da-160">In the **Identifier** textbox, type a URL using the following pattern: `http://www.panorama9.com/saml20/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3e2da-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3e2da-161">These values are not real.</span></span> <span data-ttu-id="3e2da-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3e2da-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3e2da-163">Obraťte se na [tým podpory Panorama9 klienta](https://support.panorama9.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e2da-163">Contact [Panorama9 Client support team](https://support.panorama9.com) to get these values.</span></span> 
 
4. <span data-ttu-id="3e2da-164">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="3e2da-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. <span data-ttu-id="3e2da-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3e2da-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3e2da-168">Na **Panorama9 konfigurace** klikněte na tlačítko **konfigurace Panorama9** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3e2da-168">On the **Panorama9 Configuration** section, click **Configure Panorama9** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3e2da-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3e2da-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_configure.png) 

5. <span data-ttu-id="3e2da-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Panorama9.</span><span class="sxs-lookup"><span data-stu-id="3e2da-171">In a different web browser window, log into your Panorama9 company site as an administrator.</span></span>

6. <span data-ttu-id="3e2da-172">Na panelu nástrojů v horní části klikněte na tlačítko **spravovat**a potom klikněte na **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-172">In the toolbar on the top, click **Manage**, and then click **Extensions**.</span></span>
   
   <span data-ttu-id="3e2da-173">![Rozšíření](./media/active-directory-saas-panorama9-tutorial/ic790023.png "rozšíření")</span><span class="sxs-lookup"><span data-stu-id="3e2da-173">![Extensions](./media/active-directory-saas-panorama9-tutorial/ic790023.png "Extensions")</span></span>
7. <span data-ttu-id="3e2da-174">Na **rozšíření** dialogové okno, klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-174">On the **Extensions** dialog, click **Single Sign-On**.</span></span>
   
   <span data-ttu-id="3e2da-175">![Jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/ic790024.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="3e2da-175">![Single Sign-On](./media/active-directory-saas-panorama9-tutorial/ic790024.png "Single Sign-On")</span></span>
8. <span data-ttu-id="3e2da-176">V **nastavení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e2da-176">In the **Settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="3e2da-177">![Nastavení](./media/active-directory-saas-panorama9-tutorial/ic790025.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="3e2da-177">![Settings](./media/active-directory-saas-panorama9-tutorial/ic790025.png "Settings")</span></span>
   
    <span data-ttu-id="3e2da-178">a.</span><span class="sxs-lookup"><span data-stu-id="3e2da-178">a.</span></span> <span data-ttu-id="3e2da-179">V **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e2da-179">In **Identity provider URL** textbox, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="3e2da-180">b.</span><span class="sxs-lookup"><span data-stu-id="3e2da-180">b.</span></span> <span data-ttu-id="3e2da-181">V **otisků prstů certifikátů** textovému poli, Vložit **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e2da-181">In **Certificate fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>    
         
9. <span data-ttu-id="3e2da-182">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-182">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3e2da-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3e2da-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3e2da-184">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3e2da-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3e2da-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3e2da-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3e2da-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e2da-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="3e2da-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e2da-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3e2da-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e2da-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3e2da-190">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3e2da-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3e2da-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3e2da-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e2da-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3e2da-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e2da-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3e2da-198">a.</span><span class="sxs-lookup"><span data-stu-id="3e2da-198">a.</span></span> <span data-ttu-id="3e2da-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3e2da-200">b.</span><span class="sxs-lookup"><span data-stu-id="3e2da-200">b.</span></span> <span data-ttu-id="3e2da-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3e2da-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3e2da-202">c.</span><span class="sxs-lookup"><span data-stu-id="3e2da-202">c.</span></span> <span data-ttu-id="3e2da-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3e2da-204">d.</span><span class="sxs-lookup"><span data-stu-id="3e2da-204">d.</span></span> <span data-ttu-id="3e2da-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-205">Click **Create**.</span></span>
 
### <a name="creating-a-panorama9-test-user"></a><span data-ttu-id="3e2da-206">Vytvoření zkušebního uživatele Panorama9</span><span class="sxs-lookup"><span data-stu-id="3e2da-206">Creating a Panorama9 test user</span></span>

<span data-ttu-id="3e2da-207">Pokud chcete povolit uživatelům Azure AD přihlášení do Panorama9, musí být zřízená do Panorama9.</span><span class="sxs-lookup"><span data-stu-id="3e2da-207">In order to enable Azure AD users to log into Panorama9, they must be provisioned into Panorama9.</span></span>  

<span data-ttu-id="3e2da-208">V případě Panorama9 zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="3e2da-208">In the case of Panorama9, provisioning is a manual task.</span></span>

<span data-ttu-id="3e2da-209">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e2da-209">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="3e2da-210">Přihlaste se k vaší **Panorama9** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="3e2da-210">Log in to your **Panorama9** company site as an administrator.</span></span>

2. <span data-ttu-id="3e2da-211">V nabídce v horní části, klikněte na tlačítko **spravovat**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-211">In the menu on the top, click **Manage**, and then click **Users**.</span></span>
   
  <span data-ttu-id="3e2da-212">![Uživatelé](./media/active-directory-saas-panorama9-tutorial/ic790027.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="3e2da-212">![Users](./media/active-directory-saas-panorama9-tutorial/ic790027.png "Users")</span></span>

3. <span data-ttu-id="3e2da-213">V sekci uživatelé klikněte na tlačítko  **+**  přidat nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e2da-213">In the Users section, Click **+** to add new user.</span></span>

 <span data-ttu-id="3e2da-214">![Uživatelé](./media/active-directory-saas-panorama9-tutorial/ic790028.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="3e2da-214">![Users](./media/active-directory-saas-panorama9-tutorial/ic790028.png "Users")</span></span>

4. <span data-ttu-id="3e2da-215">Přejděte k části dat uživatele, zadejte e-mailovou adresu chcete zřídit do platného uživatele Azure Active Directory **e-mailu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3e2da-215">Go to the User data section, type the email address of a valid Azure Active Directory user you want to provision into the **Email** textbox.</span></span>

5. <span data-ttu-id="3e2da-216">Se do části uživatelé klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-216">Come to the Users section, Click **Save**.</span></span>
   
> [!NOTE]
    > <span data-ttu-id="3e2da-217">Držitel účtu Azure Active Directory obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="3e2da-217">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3e2da-218">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e2da-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3e2da-219">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Panorama9.</span><span class="sxs-lookup"><span data-stu-id="3e2da-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Panorama9.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3e2da-221">**Pokud chcete přiřadit Britta Simon Panorama9, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e2da-221">**To assign Britta Simon to Panorama9, perform the following steps:**</span></span>

1. <span data-ttu-id="3e2da-222">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3e2da-224">V seznamu aplikací vyberte **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-224">In the applications list, select **Panorama9**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_app.png) 

3. <span data-ttu-id="3e2da-226">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3e2da-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3e2da-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3e2da-228">Click **Add** button.</span></span> <span data-ttu-id="3e2da-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e2da-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3e2da-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3e2da-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3e2da-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e2da-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3e2da-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e2da-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3e2da-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e2da-234">Testing single sign-on</span></span>

<span data-ttu-id="3e2da-235">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3e2da-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3e2da-236">Když kliknete na dlaždici Panorama9 na přístupovém panelu, jste měli získat automaticky přihlášení k Panorama9 aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e2da-236">When you click the Panorama9 tile in the Access Panel, you should get automatically signed-on to Panorama9 application.</span></span>
<span data-ttu-id="3e2da-237">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3e2da-237">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3e2da-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3e2da-238">Additional resources</span></span>

* [<span data-ttu-id="3e2da-239">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e2da-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3e2da-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3e2da-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_203.png

