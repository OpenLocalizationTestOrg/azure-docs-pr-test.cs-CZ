---
title: "Kurz: Azure Active Directory integrace s plátno pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a plátno pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 2212b7a81b66d1afd1aa78d1487b07b6d7b84129
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a><span data-ttu-id="87715-103">Kurz: Azure Active Directory integrace s plátno pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="87715-103">Tutorial: Azure Active Directory integration with Canvas LMS</span></span>

<span data-ttu-id="87715-104">V tomto kurzu zjistěte, jak integrovat plátno s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="87715-104">In this tutorial, you learn how to integrate Canvas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="87715-105">Integrace plátno s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="87715-105">Integrating Canvas with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="87715-106">Můžete řídit ve službě Azure AD, který má přístup na plátno</span><span class="sxs-lookup"><span data-stu-id="87715-106">You can control in Azure AD who has access to Canvas</span></span>
- <span data-ttu-id="87715-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k plátno (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="87715-107">You can enable your users to automatically get signed-on to Canvas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="87715-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="87715-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="87715-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="87715-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87715-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87715-110">Prerequisites</span></span>

<span data-ttu-id="87715-111">Konfigurace integrace Azure AD s plátno, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="87715-111">To configure Azure AD integration with Canvas, you need the following items:</span></span>

- <span data-ttu-id="87715-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="87715-112">An Azure AD subscription</span></span>
- <span data-ttu-id="87715-113">Plátně jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="87715-113">A Canvas single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="87715-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="87715-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="87715-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="87715-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="87715-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="87715-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="87715-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87715-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="87715-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="87715-118">Scenario description</span></span>
<span data-ttu-id="87715-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="87715-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="87715-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="87715-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="87715-121">Přidání plátno z Galerie</span><span class="sxs-lookup"><span data-stu-id="87715-121">Adding Canvas from the gallery</span></span>
2. <span data-ttu-id="87715-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="87715-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-canvas-from-the-gallery"></a><span data-ttu-id="87715-123">Přidání plátno z Galerie</span><span class="sxs-lookup"><span data-stu-id="87715-123">Adding Canvas from the gallery</span></span>
<span data-ttu-id="87715-124">Při konfiguraci integrace plátno do služby Azure AD potřebujete přidat plátno z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="87715-124">To configure the integration of Canvas into Azure AD, you need to add Canvas from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="87715-125">**Pokud chcete přidat plátno z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87715-125">**To add Canvas from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="87715-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="87715-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="87715-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="87715-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="87715-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="87715-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="87715-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87715-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="87715-133">Do vyhledávacího pole zadejte **plátno**.</span><span class="sxs-lookup"><span data-stu-id="87715-133">In the search box, type **Canvas**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. <span data-ttu-id="87715-135">Na panelu výsledků vyberte **plátno**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="87715-135">In the results panel, select **Canvas**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="87715-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="87715-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="87715-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování plátno podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="87715-138">In this section, you configure and test Azure AD single sign-on with Canvas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="87715-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v plátno je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="87715-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Canvas is to a user in Azure AD.</span></span> <span data-ttu-id="87715-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské plátno musí navázat.</span><span class="sxs-lookup"><span data-stu-id="87715-140">In other words, a link relationship between an Azure AD user and the related user in Canvas needs to be established.</span></span>

<span data-ttu-id="87715-141">V plátně přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="87715-141">In Canvas, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="87715-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování plátno, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="87715-142">To configure and test Azure AD single sign-on with Canvas, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="87715-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="87715-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="87715-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87715-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="87715-145">**[Vytvoření zkušebního uživatele plátno](#creating-a-canvas-test-user)**  – Pokud chcete mít protějšek Britta Simon plátno propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="87715-145">**[Creating a Canvas test user](#creating-a-canvas-test-user)** - to have a counterpart of Britta Simon in Canvas that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="87715-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="87715-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="87715-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87715-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="87715-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="87715-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="87715-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci plátno.</span><span class="sxs-lookup"><span data-stu-id="87715-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Canvas application.</span></span>

<span data-ttu-id="87715-150">**Ke konfiguraci Azure AD jednotné přihlašování plátno, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87715-150">**To configure Azure AD single sign-on with Canvas, perform the following steps:**</span></span>

1. <span data-ttu-id="87715-151">Na portálu Azure na **plátno** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="87715-151">In the Azure portal, on the **Canvas** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="87715-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="87715-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. <span data-ttu-id="87715-155">Na **plátno domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87715-155">On the **Canvas Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    <span data-ttu-id="87715-157">a.</span><span class="sxs-lookup"><span data-stu-id="87715-157">a.</span></span> <span data-ttu-id="87715-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.instructure.com`</span><span class="sxs-lookup"><span data-stu-id="87715-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.instructure.com`</span></span>

    <span data-ttu-id="87715-159">b.</span><span class="sxs-lookup"><span data-stu-id="87715-159">b.</span></span> <span data-ttu-id="87715-160">V **identifikátor** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<tenant-name>.instructure.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="87715-160">In the **Identifier** textbox, type the value using the following pattern: `https://<tenant-name>.instructure.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="87715-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="87715-161">These values are not real.</span></span> <span data-ttu-id="87715-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="87715-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="87715-163">Obraťte se na [tým podpory plátno klienta](https://community.canvaslms.com/community/help) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="87715-163">Contact [Canvas Client support team](https://community.canvaslms.com/community/help) to get these values.</span></span> 
 
4. <span data-ttu-id="87715-164">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="87715-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. <span data-ttu-id="87715-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87715-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="87715-168">Na **plátno konfigurace** klikněte na tlačítko **konfigurace plátno** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="87715-168">On the **Canvas Configuration** section, click **Configure Canvas** to open **Configure sign-on** window.</span></span> <span data-ttu-id="87715-169">Kopírování **heslo změnit adresu URL, adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="87715-169">Copy the **Change Password URL, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. <span data-ttu-id="87715-171">V okně prohlížeče jiný web Přihlaste se na váš web společnosti plátno jako správce.</span><span class="sxs-lookup"><span data-stu-id="87715-171">In a different web browser window, log in to your Canvas company site as an administrator.</span></span>

8. <span data-ttu-id="87715-172">Přejděte na **kurzy \> účty spravované \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="87715-172">Go to **Courses \> Managed Accounts \> Microsoft**.</span></span>
   
    <span data-ttu-id="87715-173">![Plátno](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "plátno")</span><span class="sxs-lookup"><span data-stu-id="87715-173">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

9. <span data-ttu-id="87715-174">V navigačním podokně na levé straně vyberte **ověřování**a potom klikněte na **přidat nová konfigurace SAML**.</span><span class="sxs-lookup"><span data-stu-id="87715-174">In the navigation pane on the left, select **Authentication**, and then click **Add New SAML Config**.</span></span>
   
    <span data-ttu-id="87715-175">![Ověřování](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="87715-175">![Authentication](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentication")</span></span>

10. <span data-ttu-id="87715-176">Na stránce aktuální integrace proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87715-176">On the Current Integration page, perform the following steps:</span></span>
   
    <span data-ttu-id="87715-177">![Aktuální integrace](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuální integrace")</span><span class="sxs-lookup"><span data-stu-id="87715-177">![Current Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Current Integration")</span></span>

    <span data-ttu-id="87715-178">a.</span><span class="sxs-lookup"><span data-stu-id="87715-178">a.</span></span> <span data-ttu-id="87715-179">V **IdP Entity ID** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87715-179">In **IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="87715-180">b.</span><span class="sxs-lookup"><span data-stu-id="87715-180">b.</span></span> <span data-ttu-id="87715-181">V **protokolu na adresu URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87715-181">In **Log On URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal .</span></span>

    <span data-ttu-id="87715-182">c.</span><span class="sxs-lookup"><span data-stu-id="87715-182">c.</span></span> <span data-ttu-id="87715-183">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87715-183">In **Log Out URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="87715-184">d.</span><span class="sxs-lookup"><span data-stu-id="87715-184">d.</span></span> <span data-ttu-id="87715-185">V **odkaz změnit heslo** textovému poli, vložte hodnotu **heslo změnit adresu URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87715-185">In **Change Password Link** textbox, paste the value of **Change Password URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="87715-186">e.</span><span class="sxs-lookup"><span data-stu-id="87715-186">e.</span></span> <span data-ttu-id="87715-187">V **otisků prstů certifikátů** textovému poli, Vložit **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87715-187">In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>      
        
    <span data-ttu-id="87715-188">f.</span><span class="sxs-lookup"><span data-stu-id="87715-188">f.</span></span> <span data-ttu-id="87715-189">Z **přihlášení atribut** seznamu, vyberte **NameID**.</span><span class="sxs-lookup"><span data-stu-id="87715-189">From the **Login Attribute** list, select **NameID**.</span></span>

    <span data-ttu-id="87715-190">g.</span><span class="sxs-lookup"><span data-stu-id="87715-190">g.</span></span> <span data-ttu-id="87715-191">Z **identifikátor formátu** seznamu, vyberte **emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="87715-191">From the **Identifier Format** list, select **emailAddress**.</span></span>

    <span data-ttu-id="87715-192">h.</span><span class="sxs-lookup"><span data-stu-id="87715-192">h.</span></span> <span data-ttu-id="87715-193">Klikněte na tlačítko **uložit nastavení ověřování**.</span><span class="sxs-lookup"><span data-stu-id="87715-193">Click **Save Authentication Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="87715-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="87715-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="87715-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="87715-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="87715-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="87715-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="87715-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="87715-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="87715-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87715-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="87715-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87715-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="87715-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="87715-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="87715-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="87715-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="87715-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87715-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="87715-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87715-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="87715-209">a.</span><span class="sxs-lookup"><span data-stu-id="87715-209">a.</span></span> <span data-ttu-id="87715-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87715-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="87715-211">b.</span><span class="sxs-lookup"><span data-stu-id="87715-211">b.</span></span> <span data-ttu-id="87715-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="87715-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="87715-213">c.</span><span class="sxs-lookup"><span data-stu-id="87715-213">c.</span></span> <span data-ttu-id="87715-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="87715-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="87715-215">d.</span><span class="sxs-lookup"><span data-stu-id="87715-215">d.</span></span> <span data-ttu-id="87715-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="87715-216">Click **Create**.</span></span>
 
### <a name="creating-a-canvas-test-user"></a><span data-ttu-id="87715-217">Vytvoření zkušebního uživatele plátno</span><span class="sxs-lookup"><span data-stu-id="87715-217">Creating a Canvas test user</span></span>

<span data-ttu-id="87715-218">Pokud chcete povolit uživatelům Azure AD přihlášení na plátno, musí být zřízená plátno.</span><span class="sxs-lookup"><span data-stu-id="87715-218">To enable Azure AD users to log in to Canvas, they must be provisioned into Canvas.</span></span>

<span data-ttu-id="87715-219">V případě plátně zřizování uživatelů je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="87715-219">In case of Canvas, user provisioning is a manual task.</span></span>

<span data-ttu-id="87715-220">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87715-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="87715-221">Přihlaste se k vaší **plátno** klienta.</span><span class="sxs-lookup"><span data-stu-id="87715-221">Log in to your **Canvas** tenant.</span></span>

2. <span data-ttu-id="87715-222">Přejděte na **kurzy \> účty spravované \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="87715-222">Go to **Courses \> Managed Accounts \> Microsoft**.</span></span>
   
   <span data-ttu-id="87715-223">![Plátno](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "plátno")</span><span class="sxs-lookup"><span data-stu-id="87715-223">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

3. <span data-ttu-id="87715-224">Klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="87715-224">Click **Users**.</span></span>
   
   <span data-ttu-id="87715-225">![Uživatelé](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="87715-225">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Users")</span></span>

4. <span data-ttu-id="87715-226">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="87715-226">Click **Add New User**.</span></span>
   
   <span data-ttu-id="87715-227">![Uživatelé](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="87715-227">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Users")</span></span>

5. <span data-ttu-id="87715-228">Na stránce Přidat nového uživatele dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87715-228">On the Add a New User dialog page, perform the following steps:</span></span>
   
   <span data-ttu-id="87715-229">![Přidat uživatele](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="87715-229">![Add User](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Add User")</span></span>
   
   <span data-ttu-id="87715-230">a.</span><span class="sxs-lookup"><span data-stu-id="87715-230">a.</span></span> <span data-ttu-id="87715-231">V **úplný název** textovému poli, zadejte jméno uživatele, jako je **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87715-231">In the **Full Name** textbox, enter the name of user like **BrittaSimon**.</span></span>

   <span data-ttu-id="87715-232">b.</span><span class="sxs-lookup"><span data-stu-id="87715-232">b.</span></span> <span data-ttu-id="87715-233">V **e-mailu** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="87715-233">In the **Email** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="87715-234">c.</span><span class="sxs-lookup"><span data-stu-id="87715-234">c.</span></span> <span data-ttu-id="87715-235">V **přihlášení** textovému poli, zadejte uživatele Azure AD e-mailovou adresu jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="87715-235">In the **Login** textbox, enter the user’s Azure AD email address like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="87715-236">d.</span><span class="sxs-lookup"><span data-stu-id="87715-236">d.</span></span> <span data-ttu-id="87715-237">Vyberte **e-mailu uživatele o vytvoření tohoto účtu**.</span><span class="sxs-lookup"><span data-stu-id="87715-237">Select **Email the user about this account creation**.</span></span>

   <span data-ttu-id="87715-238">e.</span><span class="sxs-lookup"><span data-stu-id="87715-238">e.</span></span> <span data-ttu-id="87715-239">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="87715-239">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="87715-240">Můžete použít všechny ostatní plátno uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované plátno zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="87715-240">You can use any other Canvas user account creation tools or APIs provided by Canvas to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="87715-241">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="87715-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="87715-242">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu na plátno.</span><span class="sxs-lookup"><span data-stu-id="87715-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Canvas.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="87715-244">**Přiřadit Britta Simon na plátno, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87715-244">**To assign Britta Simon to Canvas, perform the following steps:**</span></span>

1. <span data-ttu-id="87715-245">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="87715-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="87715-247">V seznamu aplikací vyberte **plátno**.</span><span class="sxs-lookup"><span data-stu-id="87715-247">In the applications list, select **Canvas**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. <span data-ttu-id="87715-249">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="87715-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="87715-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87715-251">Click **Add** button.</span></span> <span data-ttu-id="87715-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87715-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="87715-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="87715-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="87715-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87715-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="87715-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87715-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="87715-257">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="87715-257">Testing single sign-on</span></span>

<span data-ttu-id="87715-258">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="87715-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="87715-259">Když kliknete na dlaždici plátno na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci plátno.</span><span class="sxs-lookup"><span data-stu-id="87715-259">When you click the Canvas tile in the Access Panel, you should get automatically signed-on to your Canvas application.</span></span>
<span data-ttu-id="87715-260">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="87715-260">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87715-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="87715-261">Additional resources</span></span>

* [<span data-ttu-id="87715-262">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="87715-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="87715-263">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="87715-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

