---
title: 'Kurz: Azure Active Directory integrace s PagerDuty | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PagerDuty."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: bf5263ce4d8fbc231029c101f167f4b55a921e60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a><span data-ttu-id="312a1-103">Kurz: Azure Active Directory integrace s PagerDuty</span><span class="sxs-lookup"><span data-stu-id="312a1-103">Tutorial: Azure Active Directory integration with PagerDuty</span></span>

<span data-ttu-id="312a1-104">V tomto kurzu zjistěte, jak integrovat PagerDuty s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="312a1-104">In this tutorial, you learn how to integrate PagerDuty with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="312a1-105">Integrace PagerDuty s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="312a1-105">Integrating PagerDuty with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="312a1-106">Můžete řídit ve službě Azure AD, který má přístup k PagerDuty</span><span class="sxs-lookup"><span data-stu-id="312a1-106">You can control in Azure AD who has access to PagerDuty</span></span>
- <span data-ttu-id="312a1-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k PagerDuty (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="312a1-107">You can enable your users to automatically get signed-on to PagerDuty (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="312a1-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="312a1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="312a1-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="312a1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="312a1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="312a1-110">Prerequisites</span></span>

<span data-ttu-id="312a1-111">Konfigurace integrace Azure AD s PagerDuty, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="312a1-111">To configure Azure AD integration with PagerDuty, you need the following items:</span></span>

- <span data-ttu-id="312a1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="312a1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="312a1-113">PagerDuty jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="312a1-113">A PagerDuty single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="312a1-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="312a1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="312a1-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="312a1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="312a1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="312a1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="312a1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="312a1-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="312a1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="312a1-118">Scenario description</span></span>
<span data-ttu-id="312a1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="312a1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="312a1-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="312a1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="312a1-121">Přidání PagerDuty z Galerie</span><span class="sxs-lookup"><span data-stu-id="312a1-121">Adding PagerDuty from the gallery</span></span>
2. <span data-ttu-id="312a1-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="312a1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pagerduty-from-the-gallery"></a><span data-ttu-id="312a1-123">Přidání PagerDuty z Galerie</span><span class="sxs-lookup"><span data-stu-id="312a1-123">Adding PagerDuty from the gallery</span></span>
<span data-ttu-id="312a1-124">Při konfiguraci integrace PagerDuty do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS PagerDuty z galerie.</span><span class="sxs-lookup"><span data-stu-id="312a1-124">To configure the integration of PagerDuty into Azure AD, you need to add PagerDuty from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="312a1-125">**Pokud chcete přidat PagerDuty z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="312a1-125">**To add PagerDuty from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="312a1-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="312a1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="312a1-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="312a1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="312a1-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="312a1-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="312a1-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="312a1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="312a1-133">Do vyhledávacího pole zadejte **PagerDuty**, vyberte **PagerDuty** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="312a1-133">In the search box, type **PagerDuty**, select  **PagerDuty**  from result panel then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="312a1-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="312a1-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="312a1-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PagerDuty podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="312a1-136">In this section, you configure and test Azure AD single sign-on with PagerDuty based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="312a1-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v PagerDuty je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="312a1-137">For single sign-on to work, Azure AD needs to know what the counterpart user in PagerDuty is to a user in Azure AD.</span></span> <span data-ttu-id="312a1-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v PagerDuty musí navázat.</span><span class="sxs-lookup"><span data-stu-id="312a1-138">In other words, a link relationship between an Azure AD user and the related user in PagerDuty needs to be established.</span></span>

<span data-ttu-id="312a1-139">V PagerDuty, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="312a1-139">In PagerDuty, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="312a1-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PagerDuty, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="312a1-140">To configure and test Azure AD single sign-on with PagerDuty, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="312a1-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="312a1-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="312a1-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="312a1-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="312a1-143">**[Vytvoření zkušebního uživatele PagerDuty](#create-a-pagerduty-test-user)**  – Pokud chcete mít protějšek Britta Simon v PagerDuty propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="312a1-143">**[Create a PagerDuty test user](#create-a-pagerduty-test-user)** - to have a counterpart of Britta Simon in PagerDuty that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="312a1-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="312a1-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="312a1-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="312a1-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="312a1-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="312a1-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="312a1-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="312a1-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PagerDuty application.</span></span>

<span data-ttu-id="312a1-148">**Ke konfiguraci Azure AD jednotné přihlašování s PagerDuty, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="312a1-148">**To configure Azure AD single sign-on with PagerDuty, perform the following steps:**</span></span>

1. <span data-ttu-id="312a1-149">Na portálu Azure na **PagerDuty** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="312a1-149">In the Azure portal, on the **PagerDuty** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="312a1-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="312a1-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. <span data-ttu-id="312a1-153">Na **PagerDuty domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="312a1-153">On the **PagerDuty Domain and URLs** section, perform the following steps:</span></span>

    ![PagerDuty domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_url.png)

    <span data-ttu-id="312a1-155">a.</span><span class="sxs-lookup"><span data-stu-id="312a1-155">a.</span></span> <span data-ttu-id="312a1-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="312a1-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    <span data-ttu-id="312a1-157">b.</span><span class="sxs-lookup"><span data-stu-id="312a1-157">b.</span></span> <span data-ttu-id="312a1-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="312a1-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="312a1-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="312a1-159">These values are not real.</span></span> <span data-ttu-id="312a1-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="312a1-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="312a1-161">Obraťte se na [tým podpory PagerDuty klienta](https://www.pagerduty.com/support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="312a1-161">Contact [PagerDuty Client support team](https://www.pagerduty.com/support/) to get these values.</span></span> 

4. <span data-ttu-id="312a1-162">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="312a1-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. <span data-ttu-id="312a1-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="312a1-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-pagerduty-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="312a1-166">Na **PagerDuty konfigurace** klikněte na tlačítko **konfigurace PagerDuty** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="312a1-166">On the **PagerDuty Configuration** section, click **Configure PagerDuty** to open **Configure sign-on** window.</span></span> <span data-ttu-id="312a1-167">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="312a1-167">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace PagerDuty](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. <span data-ttu-id="312a1-169">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Pagerduty.</span><span class="sxs-lookup"><span data-stu-id="312a1-169">In a different web browser window, log into your Pagerduty company site as an administrator.</span></span>

8. <span data-ttu-id="312a1-170">V nabídce v horní části, klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="312a1-170">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="312a1-171">![Nastavení účtu](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="312a1-171">![Account Settings](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "Account Settings")</span></span>

9. <span data-ttu-id="312a1-172">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="312a1-172">Click **Single Sign-on**.</span></span>
   
    <span data-ttu-id="312a1-173">![Jednotné přihlašování](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="312a1-173">![Single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "Single sign-on")</span></span>

10. <span data-ttu-id="312a1-174">Na **povolit jednotné přihlašování (SSO)** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="312a1-174">On the **Enable Single Sign-on (SSO)** page, perform the following steps:</span></span>
   
    <span data-ttu-id="312a1-175">![Povolit jednotné přihlašování](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "povolit jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="312a1-175">![Enable single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "Enable single sign-on")</span></span>
   
    <span data-ttu-id="312a1-176">a.</span><span class="sxs-lookup"><span data-stu-id="312a1-176">a.</span></span> <span data-ttu-id="312a1-177">Otevření kódovaného certifikátu kódování base-64 stáhli z portálu Azure v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textbox</span><span class="sxs-lookup"><span data-stu-id="312a1-177">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>
  
    <span data-ttu-id="312a1-178">b.</span><span class="sxs-lookup"><span data-stu-id="312a1-178">b.</span></span> <span data-ttu-id="312a1-179">V **přihlašovací adresa URL** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="312a1-179">In the **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="312a1-180">c.</span><span class="sxs-lookup"><span data-stu-id="312a1-180">c.</span></span> <span data-ttu-id="312a1-181">V **adresy URL odhlašovací** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="312a1-181">In the **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="312a1-182">d.</span><span class="sxs-lookup"><span data-stu-id="312a1-182">d.</span></span> <span data-ttu-id="312a1-183">Vyberte **zapnout Single Sign-on**.</span><span class="sxs-lookup"><span data-stu-id="312a1-183">Select **Turn on Single Sign-on**.</span></span>
 
    <span data-ttu-id="312a1-184">e.</span><span class="sxs-lookup"><span data-stu-id="312a1-184">e.</span></span> <span data-ttu-id="312a1-185">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="312a1-185">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="312a1-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="312a1-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="312a1-187">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="312a1-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="312a1-188">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="312a1-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="312a1-189">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="312a1-189">Create an Azure AD test user</span></span>

<span data-ttu-id="312a1-190">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="312a1-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="312a1-192">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="312a1-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="312a1-193">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="312a1-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="312a1-195">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="312a1-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="312a1-197">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="312a1-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="312a1-199">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="312a1-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="312a1-201">a.</span><span class="sxs-lookup"><span data-stu-id="312a1-201">a.</span></span> <span data-ttu-id="312a1-202">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="312a1-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="312a1-203">b.</span><span class="sxs-lookup"><span data-stu-id="312a1-203">b.</span></span> <span data-ttu-id="312a1-204">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="312a1-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="312a1-205">c.</span><span class="sxs-lookup"><span data-stu-id="312a1-205">c.</span></span> <span data-ttu-id="312a1-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="312a1-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="312a1-207">d.</span><span class="sxs-lookup"><span data-stu-id="312a1-207">d.</span></span> <span data-ttu-id="312a1-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="312a1-208">Click **Create**.</span></span>
 
### <a name="create-a-pagerduty-test-user"></a><span data-ttu-id="312a1-209">Vytvoření zkušebního uživatele PagerDuty</span><span class="sxs-lookup"><span data-stu-id="312a1-209">Create a PagerDuty test user</span></span>

<span data-ttu-id="312a1-210">Pokud chcete povolit uživatelům Azure AD přihlášení k PagerDuty, musí být zřízená do PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="312a1-210">To enable Azure AD users to log in to PagerDuty, they must be provisioned into PagerDuty.</span></span>  
<span data-ttu-id="312a1-211">V případě PagerDuty zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="312a1-211">In the case of PagerDuty, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="312a1-212">Můžete použít všechny ostatní Pagerduty uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Pagerduty zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="312a1-212">You can use any other Pagerduty user account creation tools or APIs provided by Pagerduty to provision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="312a1-213">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="312a1-213">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="312a1-214">Přihlaste se k vaší **Pagerduty** klienta.</span><span class="sxs-lookup"><span data-stu-id="312a1-214">Log in to your **Pagerduty** tenant.</span></span>

2. <span data-ttu-id="312a1-215">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="312a1-215">In the menu on the top, click **Users**.</span></span>

3. <span data-ttu-id="312a1-216">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="312a1-216">Click **Add Users**.</span></span>
   
    <span data-ttu-id="312a1-217">![Přidání uživatelů](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="312a1-217">![Add Users](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "Add Users")</span></span>

4.  <span data-ttu-id="312a1-218">Na **pozvat váš tým** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="312a1-218">On the **Invite your team** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="312a1-219">![Pozvěte váš tým](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "pozvat váš tým")</span><span class="sxs-lookup"><span data-stu-id="312a1-219">![Invite your team](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "Invite your team")</span></span>

    <span data-ttu-id="312a1-220">a.</span><span class="sxs-lookup"><span data-stu-id="312a1-220">a.</span></span> <span data-ttu-id="312a1-221">Typ **první a poslední název** uživatele jako **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="312a1-221">Type the **First and Last Name** of user like **Britta Simon**.</span></span> 
   
    <span data-ttu-id="312a1-222">b.</span><span class="sxs-lookup"><span data-stu-id="312a1-222">b.</span></span> <span data-ttu-id="312a1-223">Zadejte **e-mailu** adresu uživatele, například  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="312a1-223">Enter **Email** address of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="312a1-224">c.</span><span class="sxs-lookup"><span data-stu-id="312a1-224">c.</span></span> <span data-ttu-id="312a1-225">Klikněte na tlačítko **přidat**a potom klikněte na **odesílání žádostí**.</span><span class="sxs-lookup"><span data-stu-id="312a1-225">Click **Add**, and then click **Send Invites**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="312a1-226">Všechny přidaní uživatelé dostanou pozvání k vytvoření účtu PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="312a1-226">All added users will receive an invite to create a PagerDuty account.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="312a1-227">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="312a1-227">Assign the Azure AD test user</span></span>

<span data-ttu-id="312a1-228">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="312a1-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PagerDuty.</span></span>

![Přiřadit role uživatele][200]

<span data-ttu-id="312a1-230">**Pokud chcete přiřadit Britta Simon PagerDuty, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="312a1-230">**To assign Britta Simon to PagerDuty, perform the following steps:**</span></span>

1. <span data-ttu-id="312a1-231">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="312a1-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="312a1-233">V seznamu aplikací vyberte **PagerDuty**.</span><span class="sxs-lookup"><span data-stu-id="312a1-233">In the applications list, select **PagerDuty**.</span></span>

    ![V seznamu aplikací na PagerDuty odkaz](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. <span data-ttu-id="312a1-235">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="312a1-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="312a1-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="312a1-237">Click **Add** button.</span></span> <span data-ttu-id="312a1-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="312a1-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="312a1-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="312a1-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="312a1-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="312a1-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="312a1-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="312a1-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="312a1-243">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="312a1-243">Test single sign-on</span></span>

<span data-ttu-id="312a1-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="312a1-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="312a1-245">Po kliknutí na tlačítko dlaždici PagerDuty v Panelyou přístup by měl získat automaticky přihlášení k aplikaci PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="312a1-245">When you click the PagerDuty tile in the Access Panelyou should get automatically signed-on to your PagerDuty application.</span></span>

<span data-ttu-id="312a1-246">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="312a1-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="312a1-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="312a1-247">Additional resources</span></span>

* [<span data-ttu-id="312a1-248">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="312a1-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="312a1-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="312a1-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_203.png

