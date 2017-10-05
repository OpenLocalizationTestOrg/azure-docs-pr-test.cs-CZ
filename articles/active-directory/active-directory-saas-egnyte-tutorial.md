---
title: 'Kurz: Azure Active Directory integrace s Egnyte | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Egnyte."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 62d01333b61e73c83588d2d1701c0c300df4ab1c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a><span data-ttu-id="0d0de-103">Kurz: Azure Active Directory integrace s Egnyte</span><span class="sxs-lookup"><span data-stu-id="0d0de-103">Tutorial: Azure Active Directory integration with Egnyte</span></span>

<span data-ttu-id="0d0de-104">V tomto kurzu zjistěte, jak integrovat Egnyte s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0d0de-104">In this tutorial, you learn how to integrate Egnyte with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0d0de-105">Integrace Egnyte s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0d0de-105">Integrating Egnyte with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0d0de-106">Můžete řídit ve službě Azure AD, který má přístup k Egnyte</span><span class="sxs-lookup"><span data-stu-id="0d0de-106">You can control in Azure AD who has access to Egnyte</span></span>
- <span data-ttu-id="0d0de-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Egnyte (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d0de-107">You can enable your users to automatically get signed-on to Egnyte (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0d0de-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0d0de-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0d0de-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0d0de-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d0de-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0d0de-110">Prerequisites</span></span>

<span data-ttu-id="0d0de-111">Konfigurace integrace Azure AD s Egnyte, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0d0de-111">To configure Azure AD integration with Egnyte, you need the following items:</span></span>

- <span data-ttu-id="0d0de-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d0de-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0d0de-113">Egnyte jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0d0de-113">An Egnyte single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0d0de-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0d0de-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0d0de-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0d0de-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0d0de-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0d0de-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0d0de-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d0de-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0d0de-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0d0de-118">Scenario description</span></span>
<span data-ttu-id="0d0de-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0d0de-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0d0de-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0d0de-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0d0de-121">Přidání Egnyte z Galerie</span><span class="sxs-lookup"><span data-stu-id="0d0de-121">Adding Egnyte from the gallery</span></span>
2. <span data-ttu-id="0d0de-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d0de-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-egnyte-from-the-gallery"></a><span data-ttu-id="0d0de-123">Přidání Egnyte z Galerie</span><span class="sxs-lookup"><span data-stu-id="0d0de-123">Adding Egnyte from the gallery</span></span>
<span data-ttu-id="0d0de-124">Při konfiguraci integrace Egnyte do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Egnyte z galerie.</span><span class="sxs-lookup"><span data-stu-id="0d0de-124">To configure the integration of Egnyte into Azure AD, you need to add Egnyte from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0d0de-125">**Pokud chcete přidat Egnyte z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0d0de-125">**To add Egnyte from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0d0de-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0d0de-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0d0de-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0d0de-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0d0de-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d0de-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0d0de-133">Do vyhledávacího pole zadejte **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-133">In the search box, type **Egnyte**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. <span data-ttu-id="0d0de-135">Na panelu výsledků vyberte **Egnyte**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d0de-135">In the results panel, select **Egnyte**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0d0de-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d0de-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0d0de-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Egnyte podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0d0de-138">In this section, you configure and test Azure AD single sign-on with Egnyte based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0d0de-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Egnyte je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d0de-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Egnyte is to a user in Azure AD.</span></span> <span data-ttu-id="0d0de-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Egnyte musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0d0de-140">In other words, a link relationship between an Azure AD user and the related user in Egnyte needs to be established.</span></span>

<span data-ttu-id="0d0de-141">V Egnyte, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="0d0de-141">In Egnyte, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0d0de-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Egnyte, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0d0de-142">To configure and test Azure AD single sign-on with Egnyte, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0d0de-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0d0de-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0d0de-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d0de-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0d0de-145">**[Vytváření testovacího uživatele Egnyte](#creating-an-egnyte-test-user)**  – Pokud chcete mít protějšek Britta Simon v Egnyte propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0d0de-145">**[Creating an Egnyte test user](#creating-an-egnyte-test-user)** - to have a counterpart of Britta Simon in Egnyte that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0d0de-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0d0de-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0d0de-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0d0de-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0d0de-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d0de-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0d0de-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Egnyte.</span><span class="sxs-lookup"><span data-stu-id="0d0de-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Egnyte application.</span></span>

<span data-ttu-id="0d0de-150">**Ke konfiguraci Azure AD jednotné přihlašování s Egnyte, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0d0de-150">**To configure Azure AD single sign-on with Egnyte, perform the following steps:**</span></span>

1. <span data-ttu-id="0d0de-151">Na portálu Azure na **Egnyte** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-151">In the Azure portal, on the **Egnyte** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0d0de-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0d0de-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. <span data-ttu-id="0d0de-155">Na **Egnyte domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d0de-155">On the **Egnyte Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    <span data-ttu-id="0d0de-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.egnyte.com`</span><span class="sxs-lookup"><span data-stu-id="0d0de-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.egnyte.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0d0de-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="0d0de-158">This value is not real.</span></span> <span data-ttu-id="0d0de-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0d0de-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="0d0de-160">Obraťte se na [tým podpory Egnyte klienta](https://www.egnyte.com/corp/contact_egnyte.html) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0d0de-160">Contact [Egnyte Client support team](https://www.egnyte.com/corp/contact_egnyte.html) to get this value.</span></span> 
 
4. <span data-ttu-id="0d0de-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="0d0de-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. <span data-ttu-id="0d0de-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0d0de-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0d0de-165">Na **Egnyte konfigurace** klikněte na tlačítko **konfigurace Egnyte** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0d0de-165">On the **Egnyte Configuration** section, click **Configure Egnyte** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0d0de-166">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0d0de-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. <span data-ttu-id="0d0de-168">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Egnyte jako správce.</span><span class="sxs-lookup"><span data-stu-id="0d0de-168">In a different web browser window, log in to your Egnyte company site as an administrator.</span></span>

8. <span data-ttu-id="0d0de-169">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-169">Click **Settings**.</span></span>
   
   <span data-ttu-id="0d0de-170">![Nastavení](./media/active-directory-saas-egnyte-tutorial/ic787819.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="0d0de-170">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787819.png "Settings")</span></span>

9. <span data-ttu-id="0d0de-171">V nabídce klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-171">In the menu, click **Settings**.</span></span>

   <span data-ttu-id="0d0de-172">![Nastavení](./media/active-directory-saas-egnyte-tutorial/ic787820.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="0d0de-172">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787820.png "Settings")</span></span>

10. <span data-ttu-id="0d0de-173">Klikněte **konfigurace** a pak klikněte **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-173">Click the **Configuration** tab, and then click **Security**.</span></span>

    <span data-ttu-id="0d0de-174">![Zabezpečení](./media/active-directory-saas-egnyte-tutorial/ic787821.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="0d0de-174">![Security](./media/active-directory-saas-egnyte-tutorial/ic787821.png "Security")</span></span>

11. <span data-ttu-id="0d0de-175">V **ověření jednotného přihlašování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d0de-175">In the **Single Sign-On Authentication** section, perform the following steps:</span></span>

    <span data-ttu-id="0d0de-176">![Jednotné přihlašování na ověřování](./media/active-directory-saas-egnyte-tutorial/ic787822.png "jednotné přihlašování v ověřování")</span><span class="sxs-lookup"><span data-stu-id="0d0de-176">![Single Sign On Authentication](./media/active-directory-saas-egnyte-tutorial/ic787822.png "Single Sign On Authentication")</span></span>   
    
    <span data-ttu-id="0d0de-177">a.</span><span class="sxs-lookup"><span data-stu-id="0d0de-177">a.</span></span> <span data-ttu-id="0d0de-178">Jako **ověření jednotného přihlašování**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-178">As **Single sign-on authentication**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="0d0de-179">b.</span><span class="sxs-lookup"><span data-stu-id="0d0de-179">b.</span></span> <span data-ttu-id="0d0de-180">Jako **zprostředkovatele Identity**, vyberte **AzureAD**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-180">As **Identity provider**, select **AzureAD**.</span></span>
   
    <span data-ttu-id="0d0de-181">c.</span><span class="sxs-lookup"><span data-stu-id="0d0de-181">c.</span></span> <span data-ttu-id="0d0de-182">Vložení **SAML jeden přihlašování adresa URL služby** zkopírovaných z portálu Azure do **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d0de-182">Paste **SAML Single Sign-On Service URL** copied from Azure portal into the **Identity provider login URL** textbox.</span></span>
   
    <span data-ttu-id="0d0de-183">d.</span><span class="sxs-lookup"><span data-stu-id="0d0de-183">d.</span></span> <span data-ttu-id="0d0de-184">Vložení **SAML Entity ID** který jste zkopírovali z portálu Azure do **ID entity zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d0de-184">Paste **SAML Entity ID** which you have copied from Azure portal into the **Identity provider entity ID** textbox.</span></span>
      
    <span data-ttu-id="0d0de-185">e.</span><span class="sxs-lookup"><span data-stu-id="0d0de-185">e.</span></span> <span data-ttu-id="0d0de-186">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d0de-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Identity provider certificate** textbox.</span></span>
   
    <span data-ttu-id="0d0de-187">f.</span><span class="sxs-lookup"><span data-stu-id="0d0de-187">f.</span></span> <span data-ttu-id="0d0de-188">Jako **výchozí mapování uživatelů**, vyberte **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-188">As **Default user mapping**, select **Email address**.</span></span>
   
    <span data-ttu-id="0d0de-189">g.</span><span class="sxs-lookup"><span data-stu-id="0d0de-189">g.</span></span> <span data-ttu-id="0d0de-190">Jako **použít hodnotu specifické pro doménu vystavitele**, vyberte **zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-190">As **Use domain-specific issuer value**, select **disabled**.</span></span>
   
    <span data-ttu-id="0d0de-191">h.</span><span class="sxs-lookup"><span data-stu-id="0d0de-191">h.</span></span> <span data-ttu-id="0d0de-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-192">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="0d0de-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="0d0de-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0d0de-194">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0d0de-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0d0de-195">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0d0de-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0d0de-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d0de-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="0d0de-197">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d0de-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0d0de-199">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0d0de-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0d0de-200">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0d0de-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0d0de-202">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0d0de-204">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d0de-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0d0de-206">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d0de-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0d0de-208">a.</span><span class="sxs-lookup"><span data-stu-id="0d0de-208">a.</span></span> <span data-ttu-id="0d0de-209">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0d0de-210">b.</span><span class="sxs-lookup"><span data-stu-id="0d0de-210">b.</span></span> <span data-ttu-id="0d0de-211">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0d0de-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0d0de-212">c.</span><span class="sxs-lookup"><span data-stu-id="0d0de-212">c.</span></span> <span data-ttu-id="0d0de-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0d0de-214">d.</span><span class="sxs-lookup"><span data-stu-id="0d0de-214">d.</span></span> <span data-ttu-id="0d0de-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-215">Click **Create**.</span></span>
 
### <a name="creating-an-egnyte-test-user"></a><span data-ttu-id="0d0de-216">Vytváření testovacího uživatele Egnyte</span><span class="sxs-lookup"><span data-stu-id="0d0de-216">Creating an Egnyte test user</span></span>

<span data-ttu-id="0d0de-217">Pokud chcete povolit uživatelům Azure AD přihlášení k Egnyte, musí být zřízená do Egnyte.</span><span class="sxs-lookup"><span data-stu-id="0d0de-217">To enable Azure AD users to log in to Egnyte, they must be provisioned into Egnyte.</span></span> <span data-ttu-id="0d0de-218">V případě Egnyte zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="0d0de-218">In the case of Egnyte, provisioning is a manual task.</span></span>

<span data-ttu-id="0d0de-219">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0d0de-219">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="0d0de-220">Přihlaste se k vaší **Egnyte** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="0d0de-220">Log in to your **Egnyte** company site as administrator.</span></span>

2. <span data-ttu-id="0d0de-221">Přejděte na **nastavení \> uživatelé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-221">Go to **Settings \> Users & Groups**.</span></span>

3. <span data-ttu-id="0d0de-222">Klikněte na tlačítko **přidat nové uživatele**a pak vyberte typ uživatele, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="0d0de-222">Click **Add New User**, and then select the type of user you want to add.</span></span>
   
   <span data-ttu-id="0d0de-223">![Uživatelé](./media/active-directory-saas-egnyte-tutorial/ic787824.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="0d0de-223">![Users](./media/active-directory-saas-egnyte-tutorial/ic787824.png "Users")</span></span>

4. <span data-ttu-id="0d0de-224">V **nové standardní uživatel** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d0de-224">In the **New Standard User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="0d0de-225">![Nový uživatel standardní](./media/active-directory-saas-egnyte-tutorial/ic787825.png "nové standardní uživatel")</span><span class="sxs-lookup"><span data-stu-id="0d0de-225">![New Standard User](./media/active-directory-saas-egnyte-tutorial/ic787825.png "New Standard User")</span></span>   

   <span data-ttu-id="0d0de-226">a.</span><span class="sxs-lookup"><span data-stu-id="0d0de-226">a.</span></span> <span data-ttu-id="0d0de-227">Typ **e-mailu**, **uživatelské jméno**a další podrobnosti chcete zřídit platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0d0de-227">Type the **Email**, **Username**, and other details of a valid Azure Active Directory account you want to provision.</span></span>
   
   <span data-ttu-id="0d0de-228">b.</span><span class="sxs-lookup"><span data-stu-id="0d0de-228">b.</span></span> <span data-ttu-id="0d0de-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-229">Click **Save**.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="0d0de-230">Držitel účtu Azure Active Directory obdrží e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="0d0de-230">The Azure Active Directory account holder will receive a notification email.</span></span>
    >

>[!NOTE]
><span data-ttu-id="0d0de-231">Můžete použít všechny ostatní Egnyte uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Egnyte zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="0d0de-231">You can use any other Egnyte user account creation tools or APIs provided by Egnyte to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0d0de-232">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d0de-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0d0de-233">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Egnyte.</span><span class="sxs-lookup"><span data-stu-id="0d0de-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Egnyte.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0d0de-235">**Pokud chcete přiřadit Britta Simon Egnyte, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0d0de-235">**To assign Britta Simon to Egnyte, perform the following steps:**</span></span>

1. <span data-ttu-id="0d0de-236">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0d0de-238">V seznamu aplikací vyberte **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-238">In the applications list, select **Egnyte**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. <span data-ttu-id="0d0de-240">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0d0de-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0d0de-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0d0de-242">Click **Add** button.</span></span> <span data-ttu-id="0d0de-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d0de-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0d0de-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0d0de-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0d0de-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d0de-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0d0de-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d0de-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0d0de-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d0de-248">Testing single sign-on</span></span>

<span data-ttu-id="0d0de-249">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0d0de-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0d0de-250">Když kliknete na dlaždici Egnyte na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Egnyte.</span><span class="sxs-lookup"><span data-stu-id="0d0de-250">When you click the Egnyte tile in the Access Panel, you should get automatically signed-on to your Egnyte application.</span></span>
<span data-ttu-id="0d0de-251">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0d0de-251">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0d0de-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0d0de-252">Additional resources</span></span>

* [<span data-ttu-id="0d0de-253">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d0de-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0d0de-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0d0de-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

