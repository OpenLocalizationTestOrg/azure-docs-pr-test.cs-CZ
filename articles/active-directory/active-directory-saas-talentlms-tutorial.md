---
title: 'Kurz: Azure Active Directory integrace s TalentLMS | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TalentLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: f28d6fbfad9dae578a20db7218b7e3b174ed859c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a><span data-ttu-id="6f65b-103">Kurz: Azure Active Directory integrace s TalentLMS</span><span class="sxs-lookup"><span data-stu-id="6f65b-103">Tutorial: Azure Active Directory integration with TalentLMS</span></span>

<span data-ttu-id="6f65b-104">V tomto kurzu zjistěte, jak integrovat TalentLMS s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6f65b-104">In this tutorial, you learn how to integrate TalentLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6f65b-105">Integrace TalentLMS s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6f65b-105">Integrating TalentLMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6f65b-106">Můžete řídit ve službě Azure AD, který má přístup k TalentLMS</span><span class="sxs-lookup"><span data-stu-id="6f65b-106">You can control in Azure AD who has access to TalentLMS</span></span>
- <span data-ttu-id="6f65b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TalentLMS (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6f65b-107">You can enable your users to automatically get signed-on to TalentLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6f65b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6f65b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6f65b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6f65b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6f65b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6f65b-110">Prerequisites</span></span>

<span data-ttu-id="6f65b-111">Konfigurace integrace Azure AD s TalentLMS, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="6f65b-111">To configure Azure AD integration with TalentLMS, you need the following items:</span></span>

- <span data-ttu-id="6f65b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6f65b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6f65b-113">TalentLMS jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6f65b-113">A TalentLMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6f65b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6f65b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6f65b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6f65b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6f65b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6f65b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6f65b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6f65b-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6f65b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6f65b-118">Scenario description</span></span>
<span data-ttu-id="6f65b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6f65b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6f65b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6f65b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6f65b-121">Přidání TalentLMS z Galerie</span><span class="sxs-lookup"><span data-stu-id="6f65b-121">Adding TalentLMS from the gallery</span></span>
2. <span data-ttu-id="6f65b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f65b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-talentlms-from-the-gallery"></a><span data-ttu-id="6f65b-123">Přidání TalentLMS z Galerie</span><span class="sxs-lookup"><span data-stu-id="6f65b-123">Adding TalentLMS from the gallery</span></span>
<span data-ttu-id="6f65b-124">Při konfiguraci integrace TalentLMS do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS TalentLMS z galerie.</span><span class="sxs-lookup"><span data-stu-id="6f65b-124">To configure the integration of TalentLMS into Azure AD, you need to add TalentLMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6f65b-125">**Pokud chcete přidat TalentLMS z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6f65b-125">**To add TalentLMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6f65b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6f65b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6f65b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6f65b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6f65b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f65b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6f65b-133">Do vyhledávacího pole zadejte **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-133">In the search box, type **TalentLMS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_search.png)

5. <span data-ttu-id="6f65b-135">Na panelu výsledků vyberte **TalentLMS**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f65b-135">In the results panel, select **TalentLMS**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6f65b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f65b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6f65b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TalentLMS podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="6f65b-138">In this section, you configure and test Azure AD single sign-on with TalentLMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6f65b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v TalentLMS je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6f65b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in TalentLMS is to a user in Azure AD.</span></span> <span data-ttu-id="6f65b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v TalentLMS musí navázat.</span><span class="sxs-lookup"><span data-stu-id="6f65b-140">In other words, a link relationship between an Azure AD user and the related user in TalentLMS needs to be established.</span></span>

<span data-ttu-id="6f65b-141">V TalentLMS, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="6f65b-141">In TalentLMS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6f65b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TalentLMS, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="6f65b-142">To configure and test Azure AD single sign-on with TalentLMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6f65b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="6f65b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6f65b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6f65b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6f65b-145">**[Vytvoření zkušebního uživatele TalentLMS](#creating-a-talentlms-test-user)**  – Pokud chcete mít protějšek Britta Simon v TalentLMS propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6f65b-145">**[Creating a TalentLMS test user](#creating-a-talentlms-test-user)** - to have a counterpart of Britta Simon in TalentLMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6f65b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6f65b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6f65b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6f65b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6f65b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f65b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6f65b-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci TalentLMS.</span><span class="sxs-lookup"><span data-stu-id="6f65b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TalentLMS application.</span></span>

<span data-ttu-id="6f65b-150">**Ke konfiguraci Azure AD jednotné přihlašování s TalentLMS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6f65b-150">**To configure Azure AD single sign-on with TalentLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="6f65b-151">Na portálu Azure na **TalentLMS** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-151">In the Azure portal, on the **TalentLMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6f65b-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6f65b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_samlbase.png)

3. <span data-ttu-id="6f65b-155">Na **TalentLMS domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6f65b-155">On the **TalentLMS Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_url.png)

    <span data-ttu-id="6f65b-157">a.</span><span class="sxs-lookup"><span data-stu-id="6f65b-157">a.</span></span> <span data-ttu-id="6f65b-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.TalentLMSapp.com`</span><span class="sxs-lookup"><span data-stu-id="6f65b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.TalentLMSapp.com`</span></span>

    <span data-ttu-id="6f65b-159">b.</span><span class="sxs-lookup"><span data-stu-id="6f65b-159">b.</span></span> <span data-ttu-id="6f65b-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://<tenant-name>.talentlms.com`</span><span class="sxs-lookup"><span data-stu-id="6f65b-160">In the **Identifier** textbox, type a URL using the following pattern: `http://<tenant-name>.talentlms.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6f65b-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6f65b-161">These values are not real.</span></span> <span data-ttu-id="6f65b-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6f65b-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6f65b-163">Obraťte se na [tým podpory TalentLMS klienta](https://www.talentlms.com/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="6f65b-163">Contact [TalentLMS Client support team](https://www.talentlms.com/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="6f65b-164">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnotu z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6f65b-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value from the certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_certificate.png) 

5. <span data-ttu-id="6f65b-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f65b-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-talentlms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6f65b-168">Na **TalentLMS konfigurace** klikněte na tlačítko **konfigurace TalentLMS** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6f65b-168">On the **TalentLMS Configuration** section, click **Configure TalentLMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6f65b-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6f65b-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_configure.png)  

7. <span data-ttu-id="6f65b-171">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti TalentLMS jako správce.</span><span class="sxs-lookup"><span data-stu-id="6f65b-171">In a different web browser window, log in to your TalentLMS company site as an administrator.</span></span>

8. <span data-ttu-id="6f65b-172">V **účet & nastavení** klikněte na položku **uživatelé** karta.</span><span class="sxs-lookup"><span data-stu-id="6f65b-172">In the **Account & Settings** section, click the **Users** tab.</span></span>
   
    <span data-ttu-id="6f65b-173">![Účet & nastavení](./media/active-directory-saas-talentlms-tutorial/IC777296.png "účet a nastavení")</span><span class="sxs-lookup"><span data-stu-id="6f65b-173">![Account & Settings](./media/active-directory-saas-talentlms-tutorial/IC777296.png "Account & Settings")</span></span>

9. <span data-ttu-id="6f65b-174">Klikněte na tlačítko **jednotné přihlašování (SSO)**,</span><span class="sxs-lookup"><span data-stu-id="6f65b-174">Click **Single Sign-On (SSO)**,</span></span>

10. <span data-ttu-id="6f65b-175">V části jednotné přihlašování proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6f65b-175">In the Single Sign-On section, perform the following steps:</span></span>
   
    <span data-ttu-id="6f65b-176">![Jednotné přihlašování](./media/active-directory-saas-talentlms-tutorial/IC777297.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="6f65b-176">![Single Sign-On](./media/active-directory-saas-talentlms-tutorial/IC777297.png "Single Sign-On")</span></span>   

    <span data-ttu-id="6f65b-177">a.</span><span class="sxs-lookup"><span data-stu-id="6f65b-177">a.</span></span> <span data-ttu-id="6f65b-178">Z **typ jednotného přihlašování k integraci** seznamu, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-178">From the **SSO integration type** list, select **SAML 2.0**.</span></span>

    <span data-ttu-id="6f65b-179">b.</span><span class="sxs-lookup"><span data-stu-id="6f65b-179">b.</span></span> <span data-ttu-id="6f65b-180">V **zprostředkovatele Identity (IDP)** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f65b-180">In the **Identity provider (IDP)** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="6f65b-181">c.</span><span class="sxs-lookup"><span data-stu-id="6f65b-181">c.</span></span> <span data-ttu-id="6f65b-182">Vložení **kryptografický otisk** hodnotu z portálu Azure do **otisků prstů certifikátů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6f65b-182">Paste the **Thumbprint** value from Azure portal into the **Certificate fingerprint** textbox.</span></span>    

    <span data-ttu-id="6f65b-183">d.</span><span class="sxs-lookup"><span data-stu-id="6f65b-183">d.</span></span>  <span data-ttu-id="6f65b-184">V **vzdáleného přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f65b-184">In the **Remote sign-in URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="6f65b-185">e.</span><span class="sxs-lookup"><span data-stu-id="6f65b-185">e.</span></span> <span data-ttu-id="6f65b-186">V **vzdálenou adresou URL odhlašování** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f65b-186">In the **Remote sign-out URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6f65b-187">f.</span><span class="sxs-lookup"><span data-stu-id="6f65b-187">f.</span></span> <span data-ttu-id="6f65b-188">Vyplňte následující údaje:</span><span class="sxs-lookup"><span data-stu-id="6f65b-188">Fill in the following:</span></span> 

    * <span data-ttu-id="6f65b-189">V **TargetedID** textovému poli, typu`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span><span class="sxs-lookup"><span data-stu-id="6f65b-189">In the **TargetedID** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span></span>
     
    * <span data-ttu-id="6f65b-190">V **křestní jméno** textovému poli, typu`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span><span class="sxs-lookup"><span data-stu-id="6f65b-190">In the **First name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span></span>
    
    * <span data-ttu-id="6f65b-191">V **příjmení** textovému poli, typu`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span><span class="sxs-lookup"><span data-stu-id="6f65b-191">In the **Last name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span></span>
    
    * <span data-ttu-id="6f65b-192">V **e-mailu** textovému poli, typu`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="6f65b-192">In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>
    
11. <span data-ttu-id="6f65b-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-193">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="6f65b-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="6f65b-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6f65b-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="6f65b-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6f65b-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6f65b-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6f65b-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6f65b-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="6f65b-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6f65b-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6f65b-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6f65b-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6f65b-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6f65b-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6f65b-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6f65b-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f65b-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6f65b-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6f65b-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6f65b-209">a.</span><span class="sxs-lookup"><span data-stu-id="6f65b-209">a.</span></span> <span data-ttu-id="6f65b-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6f65b-211">b.</span><span class="sxs-lookup"><span data-stu-id="6f65b-211">b.</span></span> <span data-ttu-id="6f65b-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6f65b-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6f65b-213">c.</span><span class="sxs-lookup"><span data-stu-id="6f65b-213">c.</span></span> <span data-ttu-id="6f65b-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6f65b-215">d.</span><span class="sxs-lookup"><span data-stu-id="6f65b-215">d.</span></span> <span data-ttu-id="6f65b-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-216">Click **Create**.</span></span>
 
### <a name="creating-a-talentlms-test-user"></a><span data-ttu-id="6f65b-217">Vytvoření zkušebního uživatele TalentLMS</span><span class="sxs-lookup"><span data-stu-id="6f65b-217">Creating a TalentLMS test user</span></span>

<span data-ttu-id="6f65b-218">Pokud chcete povolit uživatelům Azure AD přihlášení k TalentLMS, musí být zřízená do TalentLMS.</span><span class="sxs-lookup"><span data-stu-id="6f65b-218">To enable Azure AD users to log in to TalentLMS, they must be provisioned into TalentLMS.</span></span> <span data-ttu-id="6f65b-219">V případě TalentLMS zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="6f65b-219">In the case of TalentLMS, provisioning is a manual task.</span></span>

<span data-ttu-id="6f65b-220">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6f65b-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="6f65b-221">Přihlaste se k vaší **TalentLMS** klienta.</span><span class="sxs-lookup"><span data-stu-id="6f65b-221">Log in to your **TalentLMS** tenant.</span></span>

2. <span data-ttu-id="6f65b-222">Klikněte na tlačítko **uživatelé**a potom klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-222">Click **Users**, and then click **Add User**.</span></span>

3. <span data-ttu-id="6f65b-223">Na **přidat uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6f65b-223">On the **Add user** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="6f65b-224">![Přidat uživatele](./media/active-directory-saas-talentlms-tutorial/IC777299.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="6f65b-224">![Add User](./media/active-directory-saas-talentlms-tutorial/IC777299.png "Add User")</span></span>  

    <span data-ttu-id="6f65b-225">a.</span><span class="sxs-lookup"><span data-stu-id="6f65b-225">a.</span></span> <span data-ttu-id="6f65b-226">V **křestní jméno** textovému poli, zadejte jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-226">In the **First name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="6f65b-227">b.</span><span class="sxs-lookup"><span data-stu-id="6f65b-227">b.</span></span> <span data-ttu-id="6f65b-228">V **příjmení** textovému poli, zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-228">In the **Last name** textbox, enter the last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="6f65b-229">c.</span><span class="sxs-lookup"><span data-stu-id="6f65b-229">c.</span></span> <span data-ttu-id="6f65b-230">V **e-mailová adresa** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="6f65b-230">In the **Email address** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="6f65b-231">d.</span><span class="sxs-lookup"><span data-stu-id="6f65b-231">d.</span></span> <span data-ttu-id="6f65b-232">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-232">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="6f65b-233">Můžete použít všechny ostatní TalentLMS uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TalentLMS zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="6f65b-233">You can use any other TalentLMS user account creation tools or APIs provided by TalentLMS to provision AAD user accounts.</span></span>
 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6f65b-234">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6f65b-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6f65b-235">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu TalentLMS.</span><span class="sxs-lookup"><span data-stu-id="6f65b-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TalentLMS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6f65b-237">**Pokud chcete přiřadit Britta Simon TalentLMS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6f65b-237">**To assign Britta Simon to TalentLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="6f65b-238">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6f65b-240">V seznamu aplikací vyberte **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-240">In the applications list, select **TalentLMS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_app.png) 

3. <span data-ttu-id="6f65b-242">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6f65b-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6f65b-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f65b-244">Click **Add** button.</span></span> <span data-ttu-id="6f65b-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f65b-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6f65b-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6f65b-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6f65b-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f65b-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6f65b-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6f65b-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6f65b-250">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6f65b-250">Testing single sign-on</span></span>

<span data-ttu-id="6f65b-251">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6f65b-251">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6f65b-252">Když kliknete na dlaždici TalentLMS na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci TalentLMS</span><span class="sxs-lookup"><span data-stu-id="6f65b-252">When you click the TalentLMS tile in the Access Panel, you should get automatically signed-on to your TalentLMS application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6f65b-253">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6f65b-253">Additional resources</span></span>

* [<span data-ttu-id="6f65b-254">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f65b-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6f65b-255">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6f65b-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_203.png

