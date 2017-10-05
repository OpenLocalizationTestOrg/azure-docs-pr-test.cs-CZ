---
title: 'Kurz: Azure Active Directory integrace s ITRP | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ITRP."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: fae1c7b6b0e04c1e23123d3aee7913cb3131e645
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a><span data-ttu-id="86eb7-103">Kurz: Azure Active Directory integrace s ITRP</span><span class="sxs-lookup"><span data-stu-id="86eb7-103">Tutorial: Azure Active Directory integration with ITRP</span></span>

<span data-ttu-id="86eb7-104">V tomto kurzu zjistěte, jak integrovat ITRP s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="86eb7-104">In this tutorial, you learn how to integrate ITRP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="86eb7-105">Integrace ITRP s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="86eb7-105">Integrating ITRP with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="86eb7-106">Můžete řídit ve službě Azure AD, který má přístup k ITRP</span><span class="sxs-lookup"><span data-stu-id="86eb7-106">You can control in Azure AD who has access to ITRP</span></span>
- <span data-ttu-id="86eb7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ITRP (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="86eb7-107">You can enable your users to automatically get signed-on to ITRP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="86eb7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="86eb7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="86eb7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="86eb7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86eb7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="86eb7-110">Prerequisites</span></span>

<span data-ttu-id="86eb7-111">Konfigurace integrace Azure AD s ITRP, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="86eb7-111">To configure Azure AD integration with ITRP, you need the following items:</span></span>

- <span data-ttu-id="86eb7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="86eb7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="86eb7-113">ITRP jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="86eb7-113">An ITRP single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="86eb7-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="86eb7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="86eb7-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="86eb7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="86eb7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="86eb7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="86eb7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="86eb7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="86eb7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="86eb7-118">Scenario description</span></span>
<span data-ttu-id="86eb7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="86eb7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="86eb7-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="86eb7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="86eb7-121">Přidání ITRP z Galerie</span><span class="sxs-lookup"><span data-stu-id="86eb7-121">Adding ITRP from the gallery</span></span>
2. <span data-ttu-id="86eb7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="86eb7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itrp-from-the-gallery"></a><span data-ttu-id="86eb7-123">Přidání ITRP z Galerie</span><span class="sxs-lookup"><span data-stu-id="86eb7-123">Adding ITRP from the gallery</span></span>
<span data-ttu-id="86eb7-124">Chcete-li nakonfigurovat integraci ITRP v do Azure AD, přidejte ITRP z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="86eb7-124">To configure the integration of ITRP in to Azure AD, you need to add ITRP from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="86eb7-125">**Pokud chcete přidat ITRP z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="86eb7-125">**To add ITRP from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="86eb7-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="86eb7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="86eb7-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="86eb7-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="86eb7-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="86eb7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="86eb7-133">Do vyhledávacího pole zadejte **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-133">In the search box, type **ITRP**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_search.png)

5. <span data-ttu-id="86eb7-135">Na panelu výsledků vyberte **ITRP**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="86eb7-135">In the results panel, select **ITRP**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="86eb7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="86eb7-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="86eb7-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ITRP podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="86eb7-138">In this section, you configure and test Azure AD single sign-on with ITRP based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="86eb7-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ITRP je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86eb7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ITRP is to a user in Azure AD.</span></span> <span data-ttu-id="86eb7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ITRP musí navázat.</span><span class="sxs-lookup"><span data-stu-id="86eb7-140">In other words, a link relationship between an Azure AD user and the related user in ITRP needs to be established.</span></span>

<span data-ttu-id="86eb7-141">V ITRP, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="86eb7-141">In ITRP, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="86eb7-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ITRP, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="86eb7-142">To configure and test Azure AD single sign-on with ITRP, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="86eb7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="86eb7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="86eb7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86eb7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="86eb7-145">**[Vytvoření ITRP testovací uživatel](#creating-an-itrp-test-user)**  – Pokud chcete mít protějšek Britta Simon v ITRP propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="86eb7-145">**[Creating an ITRP test user](#creating-an-itrp-test-user)** - to have a counterpart of Britta Simon in ITRP that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="86eb7-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="86eb7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="86eb7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="86eb7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="86eb7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="86eb7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="86eb7-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ITRP.</span><span class="sxs-lookup"><span data-stu-id="86eb7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ITRP application.</span></span>

<span data-ttu-id="86eb7-150">**Ke konfiguraci Azure AD jednotné přihlašování s ITRP, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="86eb7-150">**To configure Azure AD single sign-on with ITRP, perform the following steps:**</span></span>

1. <span data-ttu-id="86eb7-151">Na portálu Azure na **ITRP** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-151">In the Azure portal, on the **ITRP** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="86eb7-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="86eb7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_samlbase.png)

3. <span data-ttu-id="86eb7-155">Na **ITRP domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="86eb7-155">On the **ITRP Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_url.png)

    <span data-ttu-id="86eb7-157">a.</span><span class="sxs-lookup"><span data-stu-id="86eb7-157">a.</span></span> <span data-ttu-id="86eb7-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="86eb7-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.itrp.com`</span></span>

    <span data-ttu-id="86eb7-159">b.</span><span class="sxs-lookup"><span data-stu-id="86eb7-159">b.</span></span> <span data-ttu-id="86eb7-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="86eb7-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.itrp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="86eb7-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="86eb7-161">These values are not real.</span></span> <span data-ttu-id="86eb7-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="86eb7-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="86eb7-163">Obraťte se na [tým podpory ITRP klienta](https://www.itrp.com/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="86eb7-163">Contact [ITRP Client support team](https://www.itrp.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="86eb7-164">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="86eb7-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_certificate.png) 

5. <span data-ttu-id="86eb7-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="86eb7-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="86eb7-168">Na **ITRP konfigurace** klikněte na tlačítko **konfigurace ITRP** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="86eb7-168">On the **ITRP Configuration** section, click **Configure ITRP** to open **Configure sign-on** window.</span></span> <span data-ttu-id="86eb7-169">Kopírování **SAML jednu přihlašování služby adresu URL a adresy URL Sign-Out** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="86eb7-169">Copy the **SAML Single Sign-On Service URL and Sign-Out URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_configure.png) 

7. <span data-ttu-id="86eb7-171">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti ITRP jako správce.</span><span class="sxs-lookup"><span data-stu-id="86eb7-171">In a different web browser window, log in to your ITRP company site as an administrator.</span></span>

8. <span data-ttu-id="86eb7-172">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-172">In the toolbar on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="86eb7-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span><span class="sxs-lookup"><span data-stu-id="86eb7-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span></span>

8. <span data-ttu-id="86eb7-174">V levém navigačním podokně, vyberte **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-174">In the left navigation pane, select **Single Sign-On**.</span></span>
   
    <span data-ttu-id="86eb7-175">![Jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/ic775571.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="86eb7-175">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775571.png "Single Sign-On")</span></span>

9. <span data-ttu-id="86eb7-176">V jednotné přihlašování v konfiguračním oddílu proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="86eb7-176">In the Single Sign-On configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="86eb7-177">![Jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/ic775572.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="86eb7-177">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775572.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="86eb7-178">![Jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/ic775573.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="86eb7-178">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775573.png "Single Sign-On")</span></span>   

    <span data-ttu-id="86eb7-179">a.</span><span class="sxs-lookup"><span data-stu-id="86eb7-179">a.</span></span> <span data-ttu-id="86eb7-180">Klikněte na tlačítko **povolit**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-180">Click **Enable**.</span></span>

    <span data-ttu-id="86eb7-181">b.</span><span class="sxs-lookup"><span data-stu-id="86eb7-181">b.</span></span> <span data-ttu-id="86eb7-182">V **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="86eb7-182">In **Remote Log Out URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="86eb7-183">c.</span><span class="sxs-lookup"><span data-stu-id="86eb7-183">c.</span></span> <span data-ttu-id="86eb7-184">V **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="86eb7-184">In **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="86eb7-185">d.In **otisků prstů certifikátů** textovému poli, Vložit **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="86eb7-185">d.In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span> 
      
10. <span data-ttu-id="86eb7-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="86eb7-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="86eb7-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="86eb7-188">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="86eb7-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="86eb7-189">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="86eb7-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="86eb7-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="86eb7-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="86eb7-191">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86eb7-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="86eb7-193">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="86eb7-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="86eb7-194">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="86eb7-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="86eb7-196">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="86eb7-198">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="86eb7-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="86eb7-200">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="86eb7-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="86eb7-202">a.</span><span class="sxs-lookup"><span data-stu-id="86eb7-202">a.</span></span> <span data-ttu-id="86eb7-203">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="86eb7-204">b.</span><span class="sxs-lookup"><span data-stu-id="86eb7-204">b.</span></span> <span data-ttu-id="86eb7-205">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="86eb7-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="86eb7-206">c.</span><span class="sxs-lookup"><span data-stu-id="86eb7-206">c.</span></span> <span data-ttu-id="86eb7-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="86eb7-208">d.</span><span class="sxs-lookup"><span data-stu-id="86eb7-208">d.</span></span> <span data-ttu-id="86eb7-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-209">Click **Create**.</span></span>
 
### <a name="creating-an-itrp-test-user"></a><span data-ttu-id="86eb7-210">Vytváření testovacího uživatele ITRP</span><span class="sxs-lookup"><span data-stu-id="86eb7-210">Creating an ITRP test user</span></span>

<span data-ttu-id="86eb7-211">Pokud chcete povolit uživatelům Azure AD přihlášení k ITRP, se musí být zřízená v k ITRP.</span><span class="sxs-lookup"><span data-stu-id="86eb7-211">To enable Azure AD users to log in to ITRP, they must be provisioned in to ITRP.</span></span>  

<span data-ttu-id="86eb7-212">V případě ITRP zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="86eb7-212">In the case of ITRP, provisioning is a manual task.</span></span>

<span data-ttu-id="86eb7-213">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="86eb7-213">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="86eb7-214">Přihlaste se k vaší **ITRP** klienta.</span><span class="sxs-lookup"><span data-stu-id="86eb7-214">Log in to your **ITRP** tenant.</span></span>

2. <span data-ttu-id="86eb7-215">Na panelu nástrojů v horní části klikněte na tlačítko **záznamy**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-215">In the toolbar on the top, click **Records**.</span></span>
   
    <span data-ttu-id="86eb7-216">![Správce](./media/active-directory-saas-itrp-tutorial/ic775575.png "správce")</span><span class="sxs-lookup"><span data-stu-id="86eb7-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span></span>

3. <span data-ttu-id="86eb7-217">V místní nabídce vyberte **osoby**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-217">From the popup menu, select **People**.</span></span>
   
    <span data-ttu-id="86eb7-218">![Lidé](./media/active-directory-saas-itrp-tutorial/ic775587.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="86eb7-218">![People](./media/active-directory-saas-itrp-tutorial/ic775587.png "People")</span></span>

4. <span data-ttu-id="86eb7-219">Klikněte na tlačítko **přidat nové osobě** ("+").</span><span class="sxs-lookup"><span data-stu-id="86eb7-219">Click **Add New Person** (“+”).</span></span>
   
    <span data-ttu-id="86eb7-220">![Správce](./media/active-directory-saas-itrp-tutorial/ic775576.png "správce")</span><span class="sxs-lookup"><span data-stu-id="86eb7-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span></span>

5. <span data-ttu-id="86eb7-221">V dialogovém okně Přidat nové osobě proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="86eb7-221">On the Add New Person dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="86eb7-222">![Uživatel](./media/active-directory-saas-itrp-tutorial/ic775577.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="86eb7-222">![User](./media/active-directory-saas-itrp-tutorial/ic775577.png "User")</span></span> 
      
    <span data-ttu-id="86eb7-223">a.</span><span class="sxs-lookup"><span data-stu-id="86eb7-223">a.</span></span> <span data-ttu-id="86eb7-224">Typ **název**, **e-mailu** platného účtu AAD chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="86eb7-224">Type the **Name**, **Email** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="86eb7-225">b.</span><span class="sxs-lookup"><span data-stu-id="86eb7-225">b.</span></span> <span data-ttu-id="86eb7-226">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-226">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="86eb7-227">Můžete použít všechny ostatní ITRP uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ITRP zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="86eb7-227">You can use any other ITRP user account creation tools or APIs provided by ITRP to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="86eb7-228">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="86eb7-228">Assigning the Azure AD test user</span></span>

<span data-ttu-id="86eb7-229">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ITRP.</span><span class="sxs-lookup"><span data-stu-id="86eb7-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ITRP.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="86eb7-231">**Pokud chcete přiřadit Britta Simon ITRP, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="86eb7-231">**To assign Britta Simon to ITRP, perform the following steps:**</span></span>

1. <span data-ttu-id="86eb7-232">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="86eb7-234">V seznamu aplikací vyberte **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-234">In the applications list, select **ITRP**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_app.png) 

3. <span data-ttu-id="86eb7-236">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="86eb7-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="86eb7-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="86eb7-238">Click **Add** button.</span></span> <span data-ttu-id="86eb7-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="86eb7-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="86eb7-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="86eb7-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="86eb7-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="86eb7-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="86eb7-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="86eb7-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="86eb7-244">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="86eb7-244">Testing single sign-on</span></span>

<span data-ttu-id="86eb7-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="86eb7-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="86eb7-246">Když kliknete na dlaždici ITRP na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ITRP.</span><span class="sxs-lookup"><span data-stu-id="86eb7-246">When you click the ITRP tile in the Access Panel, you should get automatically signed-on to your ITRP application.</span></span>
<span data-ttu-id="86eb7-247">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="86eb7-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86eb7-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="86eb7-248">Additional resources</span></span>

* [<span data-ttu-id="86eb7-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="86eb7-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="86eb7-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="86eb7-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_203.png

