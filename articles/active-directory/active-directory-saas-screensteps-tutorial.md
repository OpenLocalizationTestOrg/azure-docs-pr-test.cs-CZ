---
title: 'Kurz: Azure Active Directory integrace s ScreenSteps | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ScreenSteps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: b6ded8ba1adf03fdccbdb7573c09fae1857c8b16
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a><span data-ttu-id="6bcfa-103">Kurz: Azure Active Directory integrace s ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="6bcfa-103">Tutorial: Azure Active Directory integration with ScreenSteps</span></span>

<span data-ttu-id="6bcfa-104">V tomto kurzu zjistěte, jak integrovat ScreenSteps s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6bcfa-104">In this tutorial, you learn how to integrate ScreenSteps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6bcfa-105">Integrace ScreenSteps s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-105">Integrating ScreenSteps with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6bcfa-106">Můžete ovládat ve službě Azure AD, který má přístup k ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-106">You can control in Azure AD who has access to ScreenSteps.</span></span>
- <span data-ttu-id="6bcfa-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ScreenSteps (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-107">You can enable your users to automatically get signed-on to ScreenSteps (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6bcfa-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="6bcfa-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6bcfa-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6bcfa-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6bcfa-110">Prerequisites</span></span>

<span data-ttu-id="6bcfa-111">Konfigurace integrace Azure AD s ScreenSteps, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-111">To configure Azure AD integration with ScreenSteps, you need the following items:</span></span>

- <span data-ttu-id="6bcfa-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6bcfa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6bcfa-113">ScreenSteps jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6bcfa-113">A ScreenSteps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6bcfa-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6bcfa-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6bcfa-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6bcfa-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6bcfa-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6bcfa-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6bcfa-118">Scenario description</span></span>
<span data-ttu-id="6bcfa-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6bcfa-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6bcfa-121">Přidání ScreenSteps z Galerie</span><span class="sxs-lookup"><span data-stu-id="6bcfa-121">Adding ScreenSteps from the gallery</span></span>
2. <span data-ttu-id="6bcfa-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bcfa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-screensteps-from-the-gallery"></a><span data-ttu-id="6bcfa-123">Přidání ScreenSteps z Galerie</span><span class="sxs-lookup"><span data-stu-id="6bcfa-123">Adding ScreenSteps from the gallery</span></span>
<span data-ttu-id="6bcfa-124">Při konfiguraci integrace ScreenSteps do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS ScreenSteps z galerie.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-124">To configure the integration of ScreenSteps into Azure AD, you need to add ScreenSteps from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6bcfa-125">**Pokud chcete přidat ScreenSteps z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bcfa-125">**To add ScreenSteps from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6bcfa-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="6bcfa-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6bcfa-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="6bcfa-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="6bcfa-133">Do vyhledávacího pole zadejte **ScreenSteps**, vyberte **ScreenSteps** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-133">In the search box, type **ScreenSteps**, select **ScreenSteps** from result panel then click **Add** button to add the application.</span></span>

    ![ScreenSteps v seznamu výsledků](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6bcfa-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bcfa-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6bcfa-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ScreenSteps podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6bcfa-136">In this section, you configure and test Azure AD single sign-on with ScreenSteps based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6bcfa-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ScreenSteps je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ScreenSteps is to a user in Azure AD.</span></span> <span data-ttu-id="6bcfa-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ScreenSteps musí navázat.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-138">In other words, a link relationship between an Azure AD user and the related user in ScreenSteps needs to be established.</span></span>

<span data-ttu-id="6bcfa-139">V ScreenSteps, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-139">In ScreenSteps, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6bcfa-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ScreenSteps, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-140">To configure and test Azure AD single sign-on with ScreenSteps, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6bcfa-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6bcfa-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6bcfa-143">**[Vytvoření zkušebního uživatele ScreenSteps](#create-a-screensteps-test-user)**  – Pokud chcete mít protějšek Britta Simon v ScreenSteps propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-143">**[Create a ScreenSteps test user](#create-a-screensteps-test-user)** - to have a counterpart of Britta Simon in ScreenSteps that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6bcfa-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6bcfa-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6bcfa-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bcfa-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6bcfa-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ScreenSteps application.</span></span>

<span data-ttu-id="6bcfa-148">**Ke konfiguraci Azure AD jednotné přihlašování s ScreenSteps, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bcfa-148">**To configure Azure AD single sign-on with ScreenSteps, perform the following steps:**</span></span>

1. <span data-ttu-id="6bcfa-149">Na portálu Azure na **ScreenSteps** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-149">In the Azure portal, on the **ScreenSteps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="6bcfa-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. <span data-ttu-id="6bcfa-153">Na **ScreenSteps domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-153">On the **ScreenSteps Domain and URLs** section, perform the following steps:</span></span>

    ![ScreenSteps domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    <span data-ttu-id="6bcfa-155">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.ScreenSteps.com`</span><span class="sxs-lookup"><span data-stu-id="6bcfa-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.ScreenSteps.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6bcfa-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-156">This value is not real.</span></span> <span data-ttu-id="6bcfa-157">Aktualizujte tuto hodnotu skutečné přihlašování adresu URL, která se vysvětluje dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-157">Update this value with the actual Sign-On URL, which is explained later in this tutorial.</span></span> 

4. <span data-ttu-id="6bcfa-158">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. <span data-ttu-id="6bcfa-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-160">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6bcfa-162">Na **ScreenSteps konfigurace** klikněte na tlačítko **konfigurace ScreenSteps** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-162">On the **ScreenSteps Configuration** section, click **Configure ScreenSteps** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6bcfa-163">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6bcfa-163">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace ScreenSteps](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. <span data-ttu-id="6bcfa-165">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-165">In a different web browser window, log into your ScreenSteps company site as an administrator.</span></span>

8. <span data-ttu-id="6bcfa-166">Klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-166">Click **Account Settings**.</span></span>

    <span data-ttu-id="6bcfa-167">![Správa účtů](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Správa účtů")</span><span class="sxs-lookup"><span data-stu-id="6bcfa-167">![Account management](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Account management")</span></span>

9. <span data-ttu-id="6bcfa-168">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-168">Click **Single Sign-on**.</span></span>

    <span data-ttu-id="6bcfa-169">![Vzdálené ověření](./media/active-directory-saas-screensteps-tutorial/ic778524.png "vzdáleného ověřování")</span><span class="sxs-lookup"><span data-stu-id="6bcfa-169">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span></span>

10. <span data-ttu-id="6bcfa-170">Klikněte na tlačítko **vytvořit Endpoint přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-170">Click **Create Single Sign-on Endpoint**.</span></span>

    <span data-ttu-id="6bcfa-171">![Vzdálené ověření](./media/active-directory-saas-screensteps-tutorial/ic778525.png "vzdáleného ověřování")</span><span class="sxs-lookup"><span data-stu-id="6bcfa-171">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span></span>

11. <span data-ttu-id="6bcfa-172">V **vytvořit jeden přihlašování koncový bod** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-172">In the **Create Single Sign-on Endpoint** section, perform the following steps:</span></span>

    <span data-ttu-id="6bcfa-173">![Vytvořit koncový bod ověřování](./media/active-directory-saas-screensteps-tutorial/ic778526.png "vytvořit koncový bod ověřování")</span><span class="sxs-lookup"><span data-stu-id="6bcfa-173">![Create an authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778526.png "Create an authentication endpoint")</span></span>
    
    <span data-ttu-id="6bcfa-174">a.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-174">a.</span></span> <span data-ttu-id="6bcfa-175">V **název** textovému poli, zadejte název.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-175">In the **Title** textbox, type a title.</span></span>
    
    <span data-ttu-id="6bcfa-176">b.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-176">b.</span></span> <span data-ttu-id="6bcfa-177">Z **režimu** seznamu, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-177">From the **Mode** list, select **SAML**.</span></span>
    
    <span data-ttu-id="6bcfa-178">c.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-178">c.</span></span> <span data-ttu-id="6bcfa-179">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-179">Click **Create**.</span></span>

12. <span data-ttu-id="6bcfa-180">**Upravit** nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-180">**Edit** the new endpoint.</span></span>

    <span data-ttu-id="6bcfa-181">![Upravit koncový bod](./media/active-directory-saas-screensteps-tutorial/ic778528.png "upravit koncový bod")</span><span class="sxs-lookup"><span data-stu-id="6bcfa-181">![Edit endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "Edit endpoint")</span></span>

13. <span data-ttu-id="6bcfa-182">V **upravit jeden přihlašování koncový bod** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-182">In the **Edit Single Sign-on Endpoint** section, perform the following steps:</span></span>

    <span data-ttu-id="6bcfa-183">![Koncový bod ověřování. vzdálený](./media/active-directory-saas-screensteps-tutorial/ic778527.png "koncový bod vzdálené ověřování.")</span><span class="sxs-lookup"><span data-stu-id="6bcfa-183">![Remote authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication endpoint")</span></span>

    <span data-ttu-id="6bcfa-184">a.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-184">a.</span></span> <span data-ttu-id="6bcfa-185">Klikněte na tlačítko **nový soubor certifikátu SAML nahrávání**a pak nahrajte certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-185">Click **Upload new SAML Certificate file**, and then upload the certificate, which you have downloaded from Azure portal.</span></span>
    
    <span data-ttu-id="6bcfa-186">b.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-186">b.</span></span> <span data-ttu-id="6bcfa-187">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure do **vzdálené adresy URL pro přihlášení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **Remote Login URL** textbox.</span></span>
    
    <span data-ttu-id="6bcfa-188">c.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-188">c.</span></span> <span data-ttu-id="6bcfa-189">Vložení **Sign-Out URL** hodnotu, kterou jste zkopírovali z portálu Azure do **URL odhlašovací stránky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-189">Paste **Sign-Out URL** value, which you have copied from the Azure portal into the **Log out URL** textbox.</span></span>
    
    <span data-ttu-id="6bcfa-190">d.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-190">d.</span></span> <span data-ttu-id="6bcfa-191">Vyberte **skupiny** přiřazovat uživatele k když jsou zřízené.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-191">Select a **Group** to assign users to when they are provisioned.</span></span>
    
    <span data-ttu-id="6bcfa-192">e.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-192">e.</span></span> <span data-ttu-id="6bcfa-193">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-193">Click **Update**.</span></span>

    <span data-ttu-id="6bcfa-194">f.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-194">f.</span></span> <span data-ttu-id="6bcfa-195">Kopírování **SAML příjemce URL** do schránky a vložte do **přihlašovací adresa URL** textového pole v **ScreenSteps domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-195">Copy the **SAML Consumer URL** to the clipboard and paste in to the **Sign-on URL** textbox in **ScreenSteps Domain and URLs** section.</span></span>
    
    <span data-ttu-id="6bcfa-196">g.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-196">g.</span></span> <span data-ttu-id="6bcfa-197">Vraťte se na **upravit Endpoint přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-197">Return to the **Edit Single Sign-on Endpoint**.</span></span>
    
    <span data-ttu-id="6bcfa-198">h.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-198">h.</span></span> <span data-ttu-id="6bcfa-199">Klikněte **nastavit jako výchozí pro účet** tlačítko použít tento koncový bod pro všechny uživatele, kteří se připojují do ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-199">Click the **Make default for account** button to use this endpoint for all users who log into ScreenSteps.</span></span> <span data-ttu-id="6bcfa-200">Případně můžete kliknout na **přidat na web** tlačítko použít tento koncový bod pro určité weby v **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-200">Alternatively you can click the **Add to Site** button to use this endpoint for specific sites in **ScreenSteps**.</span></span>

> [!TIP]
> <span data-ttu-id="6bcfa-201">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="6bcfa-201">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6bcfa-202">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-202">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6bcfa-203">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6bcfa-203">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6bcfa-204">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6bcfa-204">Create an Azure AD test user</span></span>

<span data-ttu-id="6bcfa-205">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-205">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="6bcfa-207">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bcfa-207">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6bcfa-208">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-208">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6bcfa-210">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-210">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6bcfa-212">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-212">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6bcfa-214">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bcfa-214">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6bcfa-216">a.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-216">a.</span></span> <span data-ttu-id="6bcfa-217">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-217">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6bcfa-218">b.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-218">b.</span></span> <span data-ttu-id="6bcfa-219">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-219">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="6bcfa-220">c.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-220">c.</span></span> <span data-ttu-id="6bcfa-221">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-221">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="6bcfa-222">d.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-222">d.</span></span> <span data-ttu-id="6bcfa-223">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-223">Click **Create**.</span></span>
 
### <a name="create-a-screensteps-test-user"></a><span data-ttu-id="6bcfa-224">Vytvoření zkušebního uživatele ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="6bcfa-224">Create a ScreenSteps test user</span></span>

<span data-ttu-id="6bcfa-225">V této části vytvoříte volal Britta Simon v ScreenSteps uživatele.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-225">In this section, you create a user called Britta Simon in ScreenSteps.</span></span> <span data-ttu-id="6bcfa-226">Práce s [tým podpory ScreenSteps klienta](https://www.screensteps.com/contact) přidat uživatele do ScreenSteps platformy.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-226">Work with [ScreenSteps Client support team](https://www.screensteps.com/contact) to add the users in the ScreenSteps platform.</span></span> <span data-ttu-id="6bcfa-227">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-227">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="6bcfa-228">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6bcfa-228">Assign the Azure AD test user</span></span>

<span data-ttu-id="6bcfa-229">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ScreenSteps.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="6bcfa-231">**Pokud chcete přiřadit Britta Simon ScreenSteps, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bcfa-231">**To assign Britta Simon to ScreenSteps, perform the following steps:**</span></span>

1. <span data-ttu-id="6bcfa-232">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6bcfa-234">V seznamu aplikací vyberte **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-234">In the applications list, select **ScreenSteps**.</span></span>

    ![V seznamu aplikací na ScreenSteps odkaz](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. <span data-ttu-id="6bcfa-236">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="6bcfa-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-238">Click **Add** button.</span></span> <span data-ttu-id="6bcfa-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="6bcfa-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6bcfa-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6bcfa-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6bcfa-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bcfa-244">Test single sign-on</span></span>

<span data-ttu-id="6bcfa-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6bcfa-246">Když kliknete na dlaždici ScreenSteps na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6bcfa-246">When you click the ScreenSteps tile in the Access Panel, you should get automatically signed-on to your ScreenSteps application.</span></span>
<span data-ttu-id="6bcfa-247">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6bcfa-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6bcfa-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6bcfa-248">Additional resources</span></span>

* [<span data-ttu-id="6bcfa-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6bcfa-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6bcfa-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6bcfa-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

