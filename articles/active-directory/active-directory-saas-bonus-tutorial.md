---
title: 'Kurz: Azure Active Directory integrace s Bonusly | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Bonusly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 29a88b2efdb9f0f33f7933bc654a5a0fdf589c5a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a><span data-ttu-id="3b2ac-103">Kurz: Azure Active Directory integrace s Bonusly</span><span class="sxs-lookup"><span data-stu-id="3b2ac-103">Tutorial: Azure Active Directory integration with Bonusly</span></span>

<span data-ttu-id="3b2ac-104">V tomto kurzu zjistěte, jak integrovat Bonusly s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3b2ac-104">In this tutorial, you learn how to integrate Bonusly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3b2ac-105">Integrace Bonusly s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-105">Integrating Bonusly with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3b2ac-106">Můžete řídit ve službě Azure AD, který má přístup k Bonusly</span><span class="sxs-lookup"><span data-stu-id="3b2ac-106">You can control in Azure AD who has access to Bonusly</span></span>
- <span data-ttu-id="3b2ac-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Bonusly (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b2ac-107">You can enable your users to automatically get signed-on to Bonusly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3b2ac-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3b2ac-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3b2ac-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3b2ac-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b2ac-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3b2ac-110">Prerequisites</span></span>

<span data-ttu-id="3b2ac-111">Konfigurace integrace Azure AD s Bonusly, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-111">To configure Azure AD integration with Bonusly, you need the following items:</span></span>

- <span data-ttu-id="3b2ac-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b2ac-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3b2ac-113">Bonusly jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3b2ac-113">A Bonusly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3b2ac-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3b2ac-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3b2ac-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3b2ac-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3b2ac-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3b2ac-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3b2ac-118">Scenario description</span></span>
<span data-ttu-id="3b2ac-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3b2ac-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3b2ac-121">Přidání Bonusly z Galerie</span><span class="sxs-lookup"><span data-stu-id="3b2ac-121">Adding Bonusly from the gallery</span></span>
2. <span data-ttu-id="3b2ac-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2ac-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bonusly-from-the-gallery"></a><span data-ttu-id="3b2ac-123">Přidání Bonusly z Galerie</span><span class="sxs-lookup"><span data-stu-id="3b2ac-123">Adding Bonusly from the gallery</span></span>
<span data-ttu-id="3b2ac-124">Při konfiguraci integrace Bonusly do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Bonusly z galerie.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-124">To configure the integration of Bonusly into Azure AD, you need to add Bonusly from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3b2ac-125">**Pokud chcete přidat Bonusly z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3b2ac-125">**To add Bonusly from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3b2ac-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="3b2ac-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3b2ac-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="3b2ac-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="3b2ac-133">Do vyhledávacího pole zadejte **Bonusly**, vyberte **Bonusly** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-133">In the search box, type **Bonusly**, select **Bonusly** from result panel then click **Add** button to add the application.</span></span>

    ![Bonusly v seznamu výsledků](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3b2ac-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2ac-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="3b2ac-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Bonusly podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3b2ac-136">In this section, you configure and test Azure AD single sign-on with Bonusly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3b2ac-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Bonusly je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Bonusly is to a user in Azure AD.</span></span> <span data-ttu-id="3b2ac-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Bonusly musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-138">In other words, a link relationship between an Azure AD user and the related user in Bonusly needs to be established.</span></span>

<span data-ttu-id="3b2ac-139">V Bonusly, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-139">In Bonusly, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3b2ac-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Bonusly, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-140">To configure and test Azure AD single sign-on with Bonusly, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3b2ac-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3b2ac-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3b2ac-143">**[Vytvoření Bonusly zkušebního uživatele](#create-a-bonusly-test-user)**  – Pokud chcete mít protějšek Britta Simon v Bonusly propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-143">**[Create a Bonusly test user](#create-a-bonusly-test-user)** - to have a counterpart of Britta Simon in Bonusly that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3b2ac-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3b2ac-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3b2ac-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2ac-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3b2ac-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Bonusly.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Bonusly application.</span></span>

<span data-ttu-id="3b2ac-148">**Ke konfiguraci Azure AD jednotné přihlašování s Bonusly, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3b2ac-148">**To configure Azure AD single sign-on with Bonusly, perform the following steps:**</span></span>

1. <span data-ttu-id="3b2ac-149">Na portálu Azure na **Bonusly** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-149">In the Azure portal, on the **Bonusly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3b2ac-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_samlbase.png)

3. <span data-ttu-id="3b2ac-153">Na **Bonusly domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-153">On the **Bonusly Domain and URLs** section, perform the following steps:</span></span>

    ![Bonusly domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_url.png)

    <span data-ttu-id="3b2ac-155">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://Bonus.ly/saml/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="3b2ac-155">In the **Reply URL** textbox, type a URL using the following pattern: `https://Bonus.ly/saml/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3b2ac-156">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-156">The value is not real.</span></span> <span data-ttu-id="3b2ac-157">Aktualizujte hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-157">Update the value with the actual Reply URL.</span></span> <span data-ttu-id="3b2ac-158">Obraťte se na [tým podpory Bonusly](https://Bonusly/contact) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-158">Contact [Bonusly support team](https://Bonusly/contact) to get the value.</span></span>
 
4. <span data-ttu-id="3b2ac-159">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnotu z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-159">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value from the certificate.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_certificate.png) 

5. <span data-ttu-id="3b2ac-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-bonus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3b2ac-163">Na **Bonusly konfigurace** klikněte na tlačítko **konfigurace Bonusly** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-163">On the **Bonusly Configuration** section, click **Configure Bonusly** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3b2ac-164">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3b2ac-164">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Bonusly konfigurace](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_configure.png) 

7. <span data-ttu-id="3b2ac-166">V okně jiný prohlížeč, přihlaste se k vaší **Bonusly** klienta.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-166">In a different browser window, log in to your **Bonusly** tenant.</span></span>

8. <span data-ttu-id="3b2ac-167">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**a potom vyberte **aplikace a integrace v rámci**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-167">In the toolbar on the top, click **Settings**, and then select **Integrations and apps**.</span></span>
   
    <span data-ttu-id="3b2ac-168">![Bonusly sociálních části](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="3b2ac-168">![Bonusly Social Section](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span></span>
9. <span data-ttu-id="3b2ac-169">V části **jednotné přihlašování**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-169">Under **Single Sign-On**, select **SAML**.</span></span>

10. <span data-ttu-id="3b2ac-170">Na **SAML** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-170">On the **SAML** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="3b2ac-171">![Bonusly Saml dialogové okno stránky](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="3b2ac-171">![Bonusly Saml Dialog page](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span></span>
   
    <span data-ttu-id="3b2ac-172">a.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-172">a.</span></span> <span data-ttu-id="3b2ac-173">V **IdP SSO cílová adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-173">In the **IdP SSO target URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="3b2ac-174">b.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-174">b.</span></span> <span data-ttu-id="3b2ac-175">V **IdP vystavitele** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-175">In the **IdP Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="3b2ac-176">c.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-176">c.</span></span> <span data-ttu-id="3b2ac-177">V **IdP přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-177">In the **IdP Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="3b2ac-178">d.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-178">d.</span></span> <span data-ttu-id="3b2ac-179">Vložení **kryptografický otisk** hodnota zkopírována z portálu Azure do **otisk prstu Cert** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-179">Paste the **Thumbprint** value copied from Azure portal into the **Cert Fingerprint** textbox.</span></span>
   
11. <span data-ttu-id="3b2ac-180">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-180">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3b2ac-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3b2ac-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3b2ac-182">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3b2ac-183">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3b2ac-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3b2ac-184">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b2ac-184">Create an Azure AD test user</span></span>
<span data-ttu-id="3b2ac-185">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="3b2ac-187">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3b2ac-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3b2ac-188">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-bonus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3b2ac-190">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-bonus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3b2ac-192">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-bonus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3b2ac-194">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-bonus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3b2ac-196">a.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-196">a.</span></span> <span data-ttu-id="3b2ac-197">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3b2ac-198">b.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-198">b.</span></span> <span data-ttu-id="3b2ac-199">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3b2ac-200">c.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-200">c.</span></span> <span data-ttu-id="3b2ac-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3b2ac-202">d.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-202">d.</span></span> <span data-ttu-id="3b2ac-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-203">Click **Create**.</span></span>
 
### <a name="create-a-bonusly-test-user"></a><span data-ttu-id="3b2ac-204">Vytvoření Bonusly zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="3b2ac-204">Create a Bonusly test user</span></span>

<span data-ttu-id="3b2ac-205">Pokud chcete povolit uživatelům Azure AD přihlášení k Bonusly, musí být zřízená do Bonusly.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-205">In order to enable Azure AD users to log in to Bonusly, they must be provisioned into Bonusly.</span></span> <span data-ttu-id="3b2ac-206">V případě Bonusly zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-206">In the case of Bonusly, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="3b2ac-207">Můžete použít jakékoli jiné nástroje vytvoření Bonusly uživatelského účtu nebo rozhraní API poskytované Bonusly zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-207">You can use any other Bonusly user account creation tools or APIs provided by Bonusly to provision AAD user accounts.</span></span>
>  

<span data-ttu-id="3b2ac-208">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3b2ac-208">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="3b2ac-209">V okně webového prohlížeče Přihlaste se ke klientovi Bonusly.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-209">In a web browser window, log in to your Bonusly tenant.</span></span>

2. <span data-ttu-id="3b2ac-210">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-210">Click **Settings**.</span></span>
 
    <span data-ttu-id="3b2ac-211">![Nastavení](./media/active-directory-saas-bonus-tutorial/ic781041.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="3b2ac-211">![Settings](./media/active-directory-saas-bonus-tutorial/ic781041.png "Settings")</span></span>

3. <span data-ttu-id="3b2ac-212">Klikněte **uživatelů a bonusy** kartě.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-212">Click the **Users and bonuses** tab.</span></span>
   
    <span data-ttu-id="3b2ac-213">![Uživatelé a bonusy](./media/active-directory-saas-bonus-tutorial/ic781042.png "uživatelů a bonusy")</span><span class="sxs-lookup"><span data-stu-id="3b2ac-213">![Users and bonuses](./media/active-directory-saas-bonus-tutorial/ic781042.png "Users and bonuses")</span></span>

4. <span data-ttu-id="3b2ac-214">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-214">Click **Manage Users**.</span></span>
   
    <span data-ttu-id="3b2ac-215">![Správa uživatelů](./media/active-directory-saas-bonus-tutorial/ic781043.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="3b2ac-215">![Manage Users](./media/active-directory-saas-bonus-tutorial/ic781043.png "Manage Users")</span></span>

5. <span data-ttu-id="3b2ac-216">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-216">Click **Add User**.</span></span>
   
    <span data-ttu-id="3b2ac-217">![Přidat uživatele](./media/active-directory-saas-bonus-tutorial/ic781044.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="3b2ac-217">![Add User](./media/active-directory-saas-bonus-tutorial/ic781044.png "Add User")</span></span>

6. <span data-ttu-id="3b2ac-218">Na **přidat uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3b2ac-218">On the **Add User** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="3b2ac-219">![Přidat uživatele](./media/active-directory-saas-bonus-tutorial/ic781045.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="3b2ac-219">![Add User](./media/active-directory-saas-bonus-tutorial/ic781045.png "Add User")</span></span>  

    <span data-ttu-id="3b2ac-220">a.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-220">a.</span></span> <span data-ttu-id="3b2ac-221">V **křestní jméno** textovému poli, zadejte jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-221">In the **First name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="3b2ac-222">b.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-222">b.</span></span> <span data-ttu-id="3b2ac-223">V **příjmení** textovému poli, zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-223">In the **Last name** textbox, enter the last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="3b2ac-224">c.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-224">c.</span></span> <span data-ttu-id="3b2ac-225">V **e-mailu** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="3b2ac-225">In the **Email** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="3b2ac-226">d.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-226">d.</span></span> <span data-ttu-id="3b2ac-227">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-227">Click **Save**.</span></span>
   
     >[!NOTE]
     ><span data-ttu-id="3b2ac-228">Držitel účtu Azure AD obdrží e-mail, který obsahuje odkaz pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-228">The Azure AD account holder receives an email that includes a link to confirm the account before it becomes active.</span></span>
     >  

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="3b2ac-229">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b2ac-229">Assign the Azure AD test user</span></span>

<span data-ttu-id="3b2ac-230">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Bonusly.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-230">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Bonusly.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="3b2ac-232">**Pokud chcete přiřadit Britta Simon Bonusly, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3b2ac-232">**To assign Britta Simon to Bonusly, perform the following steps:**</span></span>

1. <span data-ttu-id="3b2ac-233">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-233">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3b2ac-235">V seznamu aplikací vyberte **Bonusly**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-235">In the applications list, select **Bonusly**.</span></span>

    ![Odkaz Bonusly v seznamu aplikací](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_app.png) 

3. <span data-ttu-id="3b2ac-237">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-237">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="3b2ac-239">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-239">Click **Add** button.</span></span> <span data-ttu-id="3b2ac-240">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="3b2ac-242">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-242">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3b2ac-243">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3b2ac-244">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3b2ac-245">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3b2ac-245">Test single sign-on</span></span>

<span data-ttu-id="3b2ac-246">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-246">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3b2ac-247">Když kliknete na dlaždici Bonusly na přístupovém panelu jste měli získat automaticky přihlášení k aplikaci Bonusly.</span><span class="sxs-lookup"><span data-stu-id="3b2ac-247">When you click the Bonusly tile in the Access Panel, you should get automatically signed-on to your Bonusly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b2ac-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3b2ac-248">Additional resources</span></span>

* [<span data-ttu-id="3b2ac-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b2ac-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3b2ac-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3b2ac-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_203.png

