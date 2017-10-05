---
title: 'Kurz: Azure Active Directory integrace s Mozy Enterprise | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Mozy Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: ac73aadcb8205f24f9d2dbce5af76f53bbcb9753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a><span data-ttu-id="cfcee-103">Kurz: Azure Active Directory integrace s Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="cfcee-103">Tutorial: Azure Active Directory integration with Mozy Enterprise</span></span>

<span data-ttu-id="cfcee-104">V tomto kurzu zjistěte, jak integrovat Mozy Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cfcee-104">In this tutorial, you learn how to integrate Mozy Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cfcee-105">Integrace Mozy Enterprise s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cfcee-105">Integrating Mozy Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cfcee-106">Můžete řídit ve službě Azure AD, který má přístup do firemní sítě Mozy</span><span class="sxs-lookup"><span data-stu-id="cfcee-106">You can control in Azure AD who has access to Mozy Enterprise</span></span>
- <span data-ttu-id="cfcee-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Mozy Enterprise (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfcee-107">You can enable your users to automatically get signed-on to Mozy Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cfcee-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cfcee-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cfcee-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cfcee-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cfcee-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cfcee-110">Prerequisites</span></span>

<span data-ttu-id="cfcee-111">Konfigurace integrace Azure AD s Mozy Enterprise, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="cfcee-111">To configure Azure AD integration with Mozy Enterprise, you need the following items:</span></span>

- <span data-ttu-id="cfcee-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfcee-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cfcee-113">Mozy Enterprise jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cfcee-113">A Mozy Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cfcee-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cfcee-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cfcee-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cfcee-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cfcee-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cfcee-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cfcee-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cfcee-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cfcee-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cfcee-118">Scenario description</span></span>
<span data-ttu-id="cfcee-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cfcee-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cfcee-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cfcee-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cfcee-121">Přidání Mozy Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="cfcee-121">Adding Mozy Enterprise from the gallery</span></span>
2. <span data-ttu-id="cfcee-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cfcee-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mozy-enterprise-from-the-gallery"></a><span data-ttu-id="cfcee-123">Přidání Mozy Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="cfcee-123">Adding Mozy Enterprise from the gallery</span></span>
<span data-ttu-id="cfcee-124">Při konfiguraci integrace Mozy Enterprise do služby Azure AD potřebujete přidat Mozy Enterprise z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cfcee-124">To configure the integration of Mozy Enterprise into Azure AD, you need to add Mozy Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cfcee-125">**Pokud chcete přidat Mozy Enterprise z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cfcee-125">**To add Mozy Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cfcee-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cfcee-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cfcee-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cfcee-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cfcee-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cfcee-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cfcee-133">Do vyhledávacího pole zadejte **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-133">In the search box, type **Mozy Enterprise**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. <span data-ttu-id="cfcee-135">Na panelu výsledků vyberte **Mozy Enterprise**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cfcee-135">In the results panel, select **Mozy Enterprise**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cfcee-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cfcee-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cfcee-138">V této části můžete nakonfigurovat, testovací Azure AD jednotného přihlašování k Mozy organizace podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cfcee-138">In this section, you configure and test Azure AD single sign-on with Mozy Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cfcee-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Mozy Enterprise je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfcee-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mozy Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="cfcee-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v podniku Mozy musí navázat.</span><span class="sxs-lookup"><span data-stu-id="cfcee-140">In other words, a link relationship between an Azure AD user and the related user in Mozy Enterprise needs to be established.</span></span>

<span data-ttu-id="cfcee-141">V Mozy Enterprise, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="cfcee-141">In Mozy Enterprise, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cfcee-142">Nakonfigurovat a otestovat Azure AD jednotného přihlašování k Mozy organizace, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="cfcee-142">To configure and test Azure AD single sign-on with Mozy Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cfcee-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="cfcee-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cfcee-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cfcee-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cfcee-145">**[Vytvoření zkušebního uživatele Mozy Enterprise](#creating-a-mozy-enterprise-test-user)**  – Pokud chcete mít protějšek Britta Simon v Mozy organizace, která je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="cfcee-145">**[Creating a Mozy Enterprise test user](#creating-a-mozy-enterprise-test-user)** - to have a counterpart of Britta Simon in Mozy Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cfcee-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cfcee-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cfcee-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cfcee-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cfcee-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cfcee-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cfcee-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="cfcee-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mozy Enterprise application.</span></span>

<span data-ttu-id="cfcee-150">**Ke konfiguraci Azure AD jednotného přihlašování k Mozy organizace, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cfcee-150">**To configure Azure AD single sign-on with Mozy Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="cfcee-151">Na portálu Azure na **Mozy Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-151">In the Azure portal, on the **Mozy Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cfcee-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cfcee-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. <span data-ttu-id="cfcee-155">Na **Mozy podnikové domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cfcee-155">On the **Mozy Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    <span data-ttu-id="cfcee-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.Mozyenterprise.com`</span><span class="sxs-lookup"><span data-stu-id="cfcee-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.Mozyenterprise.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cfcee-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="cfcee-158">This value is not real.</span></span> <span data-ttu-id="cfcee-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cfcee-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="cfcee-160">Obraťte se na [tým podpory klient systému Enterprise Mozy](http://support.mozy.com/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cfcee-160">Contact [Mozy Enterprise Client support team](http://support.mozy.com/) to get this value.</span></span>

4. <span data-ttu-id="cfcee-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="cfcee-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. <span data-ttu-id="cfcee-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cfcee-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cfcee-165">Na **Mozy podniková konfigurace** klikněte na tlačítko **konfigurace Mozy Enterprise** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cfcee-165">On the **Mozy Enterprise Configuration** section, click **Configure Mozy Enterprise** to open **Configure sign-on** window.</span></span> <span data-ttu-id="cfcee-166">Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cfcee-166">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. <span data-ttu-id="cfcee-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="cfcee-168">In a different web browser window, log into your Mozy Enterprise company site as an administrator.</span></span>

8. <span data-ttu-id="cfcee-169">V **konfigurace** klikněte na tlačítko **zásady ověřování**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-169">In the **Configuration** section, click **Authentication Policy**.</span></span>
   
   <span data-ttu-id="cfcee-170">![Zásady ověřování](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "zásady ověřování")</span><span class="sxs-lookup"><span data-stu-id="cfcee-170">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "Authentication policy")</span></span>

9. <span data-ttu-id="cfcee-171">Na **zásady ověřování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cfcee-171">On the **Authentication Policy** section, perform the following steps:</span></span>
   
   <span data-ttu-id="cfcee-172">![Zásady ověřování](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "zásady ověřování")</span><span class="sxs-lookup"><span data-stu-id="cfcee-172">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "Authentication policy")</span></span>
   
   <span data-ttu-id="cfcee-173">a.</span><span class="sxs-lookup"><span data-stu-id="cfcee-173">a.</span></span> <span data-ttu-id="cfcee-174">Vyberte **adresářová služba** jako **zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-174">Select **Directory Service** as **Provider**.</span></span>
   
   <span data-ttu-id="cfcee-175">b.</span><span class="sxs-lookup"><span data-stu-id="cfcee-175">b.</span></span> <span data-ttu-id="cfcee-176">Vyberte **použít nabízenou LDAP**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-176">Select **Use LDAP Push**.</span></span>
   
   <span data-ttu-id="cfcee-177">c.</span><span class="sxs-lookup"><span data-stu-id="cfcee-177">c.</span></span> <span data-ttu-id="cfcee-178">Klikněte **ověřování SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="cfcee-178">Click the **SAML Authentication** tab.</span></span>
   
   <span data-ttu-id="cfcee-179">d.</span><span class="sxs-lookup"><span data-stu-id="cfcee-179">d.</span></span> <span data-ttu-id="cfcee-180">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do **adresy URL ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="cfcee-180">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Authentication URL** textbox.</span></span>
   
   <span data-ttu-id="cfcee-181">e.</span><span class="sxs-lookup"><span data-stu-id="cfcee-181">e.</span></span> <span data-ttu-id="cfcee-182">Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do **koncový bod SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="cfcee-182">Paste **SAML Entity ID**, which you have copied from the Azure portal into the **SAML Endpoint** textbox.</span></span>
   
   <span data-ttu-id="cfcee-183">f.</span><span class="sxs-lookup"><span data-stu-id="cfcee-183">f.</span></span> <span data-ttu-id="cfcee-184">V poznámkovém bloku otevřete stažené kódovaného certifikátu kódování base-64, zkopírujte obsah ho do schránky a vložte celý certifikát do **certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="cfcee-184">Open your downloaded base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **SAML Certificate** textbox.</span></span>
   
   <span data-ttu-id="cfcee-185">g.</span><span class="sxs-lookup"><span data-stu-id="cfcee-185">g.</span></span> <span data-ttu-id="cfcee-186">Vyberte **povolit jednotné přihlašování pro správce k přihlášení pomocí přihlašovacích údajů sítě**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-186">Select **Enable SSO for Admins to log in with their network credentials**.</span></span>
   
   <span data-ttu-id="cfcee-187">h.</span><span class="sxs-lookup"><span data-stu-id="cfcee-187">h.</span></span> <span data-ttu-id="cfcee-188">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-188">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="cfcee-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="cfcee-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cfcee-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="cfcee-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cfcee-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cfcee-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cfcee-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfcee-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="cfcee-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cfcee-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cfcee-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cfcee-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cfcee-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cfcee-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cfcee-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cfcee-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cfcee-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cfcee-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cfcee-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cfcee-204">a.</span><span class="sxs-lookup"><span data-stu-id="cfcee-204">a.</span></span> <span data-ttu-id="cfcee-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cfcee-206">b.</span><span class="sxs-lookup"><span data-stu-id="cfcee-206">b.</span></span> <span data-ttu-id="cfcee-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cfcee-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cfcee-208">c.</span><span class="sxs-lookup"><span data-stu-id="cfcee-208">c.</span></span> <span data-ttu-id="cfcee-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cfcee-210">d.</span><span class="sxs-lookup"><span data-stu-id="cfcee-210">d.</span></span> <span data-ttu-id="cfcee-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-211">Click **Create**.</span></span>
 
### <a name="creating-a-mozy-enterprise-test-user"></a><span data-ttu-id="cfcee-212">Vytvoření zkušebního uživatele Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="cfcee-212">Creating a Mozy Enterprise test user</span></span>

<span data-ttu-id="cfcee-213">Pokud chcete povolit uživatelům Azure AD přihlášení do Mozy Enterprise, musí být zřízená do Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="cfcee-213">In order to enable Azure AD users to log into Mozy Enterprise, they must be provisioned into Mozy Enterprise.</span></span> <span data-ttu-id="cfcee-214">V případě Mozy Enterprise zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="cfcee-214">In the case of Mozy Enterprise, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="cfcee-215">Můžete použít všechny ostatní Mozy Enterprise uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Mozy Enterprise zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="cfcee-215">You can use any other Mozy Enterprise user account creation tools or APIs provided by Mozy Enterprise to provision AAD user accounts.</span></span>

<span data-ttu-id="cfcee-216">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cfcee-216">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="cfcee-217">Přihlaste se k vaší **Mozy Enterprise** klienta.</span><span class="sxs-lookup"><span data-stu-id="cfcee-217">Log in to your **Mozy Enterprise** tenant.</span></span>

2. <span data-ttu-id="cfcee-218">Klikněte na tlačítko **uživatelé**a potom klikněte na **přidat nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-218">Click **Users**, and then click **Add New User**.</span></span>
   
   <span data-ttu-id="cfcee-219">![Uživatelé](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="cfcee-219">![Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "Users")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="cfcee-220">**Přidat nové uživatele** možnost se zobrazí pouze pouze v případě **Mozy** je vybrán jako zprostředkovatel pod **zásady ověřování**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-220">The **Add New User** option is only displayed only if **Mozy** is selected as the provider under **Authentication policy**.</span></span> <span data-ttu-id="cfcee-221">Pokud je nakonfigurované ověřování SAML, pak uživatelé jsou automaticky přidá na jejich první přihlášení pomocí jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="cfcee-221">If SAML Authentication is configured, then the users are added automatically on their first login through Single sign on.</span></span>
    
3. <span data-ttu-id="cfcee-222">V dialogovém okně Nový uživatel proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cfcee-222">On the new user dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="cfcee-223">![Přidání uživatelů](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="cfcee-223">![Add Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "Add Users")</span></span>
   
   <span data-ttu-id="cfcee-224">a.</span><span class="sxs-lookup"><span data-stu-id="cfcee-224">a.</span></span> <span data-ttu-id="cfcee-225">Z **zvolte skupinu** seznamu, vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="cfcee-225">From the **Choose a Group** list, select a group.</span></span>
   
   <span data-ttu-id="cfcee-226">b.</span><span class="sxs-lookup"><span data-stu-id="cfcee-226">b.</span></span> <span data-ttu-id="cfcee-227">Z **jaký typ uživatele** seznamu, vyberte typ.</span><span class="sxs-lookup"><span data-stu-id="cfcee-227">From the **What type of user** list, select a type.</span></span>
   
   <span data-ttu-id="cfcee-228">c.</span><span class="sxs-lookup"><span data-stu-id="cfcee-228">c.</span></span> <span data-ttu-id="cfcee-229">V **uživatelské jméno** textovému poli, zadejte jméno uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfcee-229">In the **Username** textbox, type the name of the Azure AD user.</span></span>
   
   <span data-ttu-id="cfcee-230">d.</span><span class="sxs-lookup"><span data-stu-id="cfcee-230">d.</span></span> <span data-ttu-id="cfcee-231">V **e-mailu** textovému poli, zadejte e-mailovou adresu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfcee-231">In the **Email** textbox, type the email address of the Azure AD user.</span></span>
   
   <span data-ttu-id="cfcee-232">e.</span><span class="sxs-lookup"><span data-stu-id="cfcee-232">e.</span></span> <span data-ttu-id="cfcee-233">Vyberte **odeslat e-mail uživatele instrukce**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-233">Select **Send user instruction email**.</span></span>
   
   <span data-ttu-id="cfcee-234">f.</span><span class="sxs-lookup"><span data-stu-id="cfcee-234">f.</span></span> <span data-ttu-id="cfcee-235">Klikněte na tlačítko **přidat jiní uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-235">Click **Add User(s)**.</span></span>

     >[!NOTE]
     > <span data-ttu-id="cfcee-236">Po vytvoření uživatele, odešle e-mailu pro uživatele Azure AD, která obsahuje odkaz pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="cfcee-236">After creating the user, an email will be sent to the Azure AD user that includes a link to confirm the account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cfcee-237">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfcee-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cfcee-238">V této části povolíte Britta Simon používat tak, že udělíte přístup do firemní sítě Mozy Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cfcee-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mozy Enterprise.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cfcee-240">**Pokud chcete přiřadit Britta Simon Mozy Enterprise, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cfcee-240">**To assign Britta Simon to Mozy Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="cfcee-241">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cfcee-243">V seznamu aplikací vyberte **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-243">In the applications list, select **Mozy Enterprise**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. <span data-ttu-id="cfcee-245">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cfcee-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cfcee-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cfcee-247">Click **Add** button.</span></span> <span data-ttu-id="cfcee-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cfcee-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cfcee-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cfcee-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cfcee-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cfcee-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cfcee-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cfcee-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cfcee-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cfcee-253">Testing single sign-on</span></span>

<span data-ttu-id="cfcee-254">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cfcee-254">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cfcee-255">Když kliknete na dlaždici Mozy Enterprise na přístupovém panelu, měli byste obdržet přihlašovací stránku Mozy podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfcee-255">When you click the Mozy Enterprise tile in the Access Panel, you should get login page of Mozy Enterprise application.</span></span>
<span data-ttu-id="cfcee-256">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cfcee-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cfcee-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cfcee-257">Additional resources</span></span>

* [<span data-ttu-id="cfcee-258">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cfcee-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cfcee-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cfcee-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png

