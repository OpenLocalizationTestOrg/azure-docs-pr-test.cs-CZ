---
title: 'Kurz: Azure Active Directory integrace s TargetProcess | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TargetProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: d15931a5d430252bbd9ae342e1f8fde1a539355b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a><span data-ttu-id="2db40-103">Kurz: Azure Active Directory integrace s TargetProcess</span><span class="sxs-lookup"><span data-stu-id="2db40-103">Tutorial: Azure Active Directory integration with TargetProcess</span></span>

<span data-ttu-id="2db40-104">V tomto kurzu zjistěte, jak integrovat TargetProcess s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2db40-104">In this tutorial, you learn how to integrate TargetProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2db40-105">Integrace TargetProcess s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2db40-105">Integrating TargetProcess with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2db40-106">Můžete řídit ve službě Azure AD, který má přístup k TargetProcess</span><span class="sxs-lookup"><span data-stu-id="2db40-106">You can control in Azure AD who has access to TargetProcess</span></span>
- <span data-ttu-id="2db40-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TargetProcess (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2db40-107">You can enable your users to automatically get signed-on to TargetProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2db40-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2db40-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2db40-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2db40-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2db40-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2db40-110">Prerequisites</span></span>

<span data-ttu-id="2db40-111">Konfigurace integrace Azure AD s TargetProcess, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2db40-111">To configure Azure AD integration with TargetProcess, you need the following items:</span></span>

- <span data-ttu-id="2db40-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2db40-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2db40-113">TargetProcess jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2db40-113">A TargetProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2db40-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2db40-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2db40-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2db40-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2db40-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2db40-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2db40-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2db40-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2db40-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2db40-118">Scenario description</span></span>
<span data-ttu-id="2db40-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2db40-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2db40-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2db40-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2db40-121">Přidat TargetProcess z Galerie</span><span class="sxs-lookup"><span data-stu-id="2db40-121">Add TargetProcess from the gallery</span></span>
2. <span data-ttu-id="2db40-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2db40-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-targetprocess-from-the-gallery"></a><span data-ttu-id="2db40-123">Přidat TargetProcess z Galerie</span><span class="sxs-lookup"><span data-stu-id="2db40-123">Add TargetProcess from the gallery</span></span>
<span data-ttu-id="2db40-124">Při konfiguraci integrace TargetProcess do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS TargetProcess z galerie.</span><span class="sxs-lookup"><span data-stu-id="2db40-124">To configure the integration of TargetProcess into Azure AD, you need to add TargetProcess from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2db40-125">**Pokud chcete přidat TargetProcess z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2db40-125">**To add TargetProcess from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2db40-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2db40-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2db40-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2db40-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2db40-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2db40-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2db40-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2db40-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2db40-133">Do vyhledávacího pole zadejte **TargetProcess**, vyberte **TargetProcess** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2db40-133">In the search box, type **TargetProcess**, select **TargetProcess**  from result panel then click **Add** button to add the application.</span></span>

    ![Přidat TargetProcess z Galerie](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="2db40-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2db40-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="2db40-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TargetProcess podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2db40-136">In this section, you configure and test Azure AD single sign-on with TargetProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2db40-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v TargetProcess je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2db40-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TargetProcess is to a user in Azure AD.</span></span> <span data-ttu-id="2db40-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v TargetProcess musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2db40-138">In other words, a link relationship between an Azure AD user and the related user in TargetProcess needs to be established.</span></span>

<span data-ttu-id="2db40-139">V TargetProcess, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="2db40-139">In TargetProcess, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2db40-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TargetProcess, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2db40-140">To configure and test Azure AD single sign-on with TargetProcess, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2db40-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2db40-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2db40-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2db40-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2db40-143">**[Vytvoření zkušebního uživatele TargetProcess](#create-a-targetprocess-test-user)**  – Pokud chcete mít protějšek Britta Simon v TargetProcess propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2db40-143">**[Create a TargetProcess test user](#create-a-targetprocess-test-user)** - to have a counterpart of Britta Simon in TargetProcess that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2db40-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2db40-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2db40-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2db40-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="2db40-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2db40-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="2db40-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="2db40-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TargetProcess application.</span></span>

<span data-ttu-id="2db40-148">**Ke konfiguraci Azure AD jednotné přihlašování s TargetProcess, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2db40-148">**To configure Azure AD single sign-on with TargetProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="2db40-149">Na portálu Azure na **TargetProcess** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2db40-149">In the Azure portal, on the **TargetProcess** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2db40-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2db40-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. <span data-ttu-id="2db40-153">Na **TargetProcess domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2db40-153">On the **TargetProcess Domain and URLs** section, perform the following steps:</span></span>

    ![Část TargetProcess domény a adresy URL](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    <span data-ttu-id="2db40-155">a.</span><span class="sxs-lookup"><span data-stu-id="2db40-155">a.</span></span> <span data-ttu-id="2db40-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="2db40-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    <span data-ttu-id="2db40-157">b.</span><span class="sxs-lookup"><span data-stu-id="2db40-157">b.</span></span> <span data-ttu-id="2db40-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="2db40-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2db40-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2db40-159">These values are not real.</span></span> <span data-ttu-id="2db40-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2db40-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2db40-161">Obraťte se na [tým podpory TargetProcess klienta](mailto:support@targetprocess.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="2db40-161">Contact [TargetProcess Client support team](mailto:support@targetprocess.com) to get these values.</span></span> 
 
4. <span data-ttu-id="2db40-162">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="2db40-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. <span data-ttu-id="2db40-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2db40-164">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2db40-166">Na **TargetProcess konfigurace** klikněte na tlačítko **konfigurace TargetProcess** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2db40-166">On the **TargetProcess Configuration** section, click **Configure TargetProcess** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2db40-167">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="2db40-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TargetProcess konfigurační oddíl](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. <span data-ttu-id="2db40-169">Přihlášení do aplikace TargetProcess jako správce.</span><span class="sxs-lookup"><span data-stu-id="2db40-169">Sign-on to your TargetProcess application as an administrator.</span></span>

8. <span data-ttu-id="2db40-170">V nabídce v horní části, klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="2db40-170">In the menu on the top, click **Setup**.</span></span>
   
    ![Nastavení](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. <span data-ttu-id="2db40-172">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2db40-172">Click **Settings**.</span></span>
   
    ![Nastavení](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. <span data-ttu-id="2db40-174">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2db40-174">Click **Single Sign-on**.</span></span>
   
    ![Klikněte na jednotné přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. <span data-ttu-id="2db40-176">Na jednotné přihlašování v dialogovém okně nastavení proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2db40-176">On the Single Sign-on settings dialog, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    <span data-ttu-id="2db40-178">a.</span><span class="sxs-lookup"><span data-stu-id="2db40-178">a.</span></span> <span data-ttu-id="2db40-179">Klikněte na tlačítko **povolit jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2db40-179">Click **Enable Single Sign-on**.</span></span>
    
    <span data-ttu-id="2db40-180">b.</span><span class="sxs-lookup"><span data-stu-id="2db40-180">b.</span></span> <span data-ttu-id="2db40-181">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2db40-181">In **Sign-on URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2db40-182">c.</span><span class="sxs-lookup"><span data-stu-id="2db40-182">c.</span></span> <span data-ttu-id="2db40-183">V poznámkovém bloku otevřete stažený certifikát, kopírovat obsah a vložte ji do **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2db40-183">Open your downloaded certificate in notepad, copy the content, and then paste it into the **Certificate** textbox.</span></span>
    
    <span data-ttu-id="2db40-184">d.</span><span class="sxs-lookup"><span data-stu-id="2db40-184">d.</span></span> <span data-ttu-id="2db40-185">Klikněte na tlačítko **povolit zřizování JIT**.</span><span class="sxs-lookup"><span data-stu-id="2db40-185">click **Enable JIT Provisioning**.</span></span>

    <span data-ttu-id="2db40-186">e.</span><span class="sxs-lookup"><span data-stu-id="2db40-186">e.</span></span> <span data-ttu-id="2db40-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2db40-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2db40-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2db40-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2db40-189">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="2db40-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2db40-190">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2db40-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="2db40-191">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2db40-191">Create an Azure AD test user</span></span>
<span data-ttu-id="2db40-192">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2db40-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2db40-194">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2db40-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2db40-195">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2db40-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2db40-197">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2db40-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Chcete-li zobrazit seznam uživatelů](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2db40-199">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2db40-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2db40-201">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2db40-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Oddíl uživatele](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2db40-203">a.</span><span class="sxs-lookup"><span data-stu-id="2db40-203">a.</span></span> <span data-ttu-id="2db40-204">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2db40-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2db40-205">b.</span><span class="sxs-lookup"><span data-stu-id="2db40-205">b.</span></span> <span data-ttu-id="2db40-206">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2db40-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2db40-207">c.</span><span class="sxs-lookup"><span data-stu-id="2db40-207">c.</span></span> <span data-ttu-id="2db40-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2db40-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2db40-209">d.</span><span class="sxs-lookup"><span data-stu-id="2db40-209">d.</span></span> <span data-ttu-id="2db40-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2db40-210">Click **Create**.</span></span>
 
### <a name="create-a-targetprocess-test-user"></a><span data-ttu-id="2db40-211">Vytvoření zkušebního uživatele TargetProcess</span><span class="sxs-lookup"><span data-stu-id="2db40-211">Create a TargetProcess test user</span></span>

<span data-ttu-id="2db40-212">Cílem této části je vytvoření uživatele v TargetProcess nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2db40-212">The objective of this section is to create a user called Britta Simon in TargetProcess.</span></span>

<span data-ttu-id="2db40-213">TargetProcess podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="2db40-213">TargetProcess supports just-in-time provisioning.</span></span> <span data-ttu-id="2db40-214">Již je v povolíte [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="2db40-214">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="2db40-215">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="2db40-215">There is no action item for you in this section.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="2db40-216">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2db40-216">Assign the Azure AD test user</span></span>

<span data-ttu-id="2db40-217">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="2db40-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TargetProcess.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2db40-219">**Pokud chcete přiřadit Britta Simon TargetProcess, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2db40-219">**To assign Britta Simon to TargetProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="2db40-220">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2db40-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2db40-222">V seznamu aplikací vyberte **TargetProcess**.</span><span class="sxs-lookup"><span data-stu-id="2db40-222">In the applications list, select **TargetProcess**.</span></span>

    ![TargetProcess v seznamu aplikací](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. <span data-ttu-id="2db40-224">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2db40-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2db40-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2db40-226">Click **Add** button.</span></span> <span data-ttu-id="2db40-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2db40-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2db40-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2db40-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2db40-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2db40-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2db40-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2db40-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="2db40-232">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2db40-232">Test single sign-on</span></span>

<span data-ttu-id="2db40-233">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2db40-233">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2db40-234">Když kliknete na dlaždici TargetProcess na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="2db40-234">When you click the TargetProcess tile in the Access Panel, you should get automatically signed-on to your TargetProcess application.</span></span> <span data-ttu-id="2db40-235">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2db40-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2db40-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2db40-236">Additional resources</span></span>

* [<span data-ttu-id="2db40-237">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2db40-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2db40-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2db40-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

