---
title: 'Kurz: Azure Active Directory integrace s New Relic. | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a New Relic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 605e85c23a849f70fcc0237361d7a891f716ca3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a><span data-ttu-id="aeeb4-103">Kurz: Azure Active Directory integrace s New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-103">Tutorial: Azure Active Directory integration with New Relic</span></span>

<span data-ttu-id="aeeb4-104">V tomto kurzu zjistěte, jak integrovat New Relic s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="aeeb4-104">In this tutorial, you learn how to integrate New Relic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aeeb4-105">Integrace New Relic s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-105">Integrating New Relic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="aeeb4-106">Můžete řídit ve službě Azure AD, který má přístup k New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-106">You can control in Azure AD who has access to New Relic</span></span>
- <span data-ttu-id="aeeb4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k New Relic (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="aeeb4-107">You can enable your users to automatically get signed-on to New Relic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="aeeb4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="aeeb4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="aeeb4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aeeb4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aeeb4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="aeeb4-110">Prerequisites</span></span>

<span data-ttu-id="aeeb4-111">Konfigurace integrace Azure AD s New Relic, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-111">To configure Azure AD integration with New Relic, you need the following items:</span></span>

- <span data-ttu-id="aeeb4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="aeeb4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="aeeb4-113">New Relic jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="aeeb4-113">A New Relic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aeeb4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aeeb4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aeeb4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aeeb4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aeeb4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aeeb4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="aeeb4-118">Scenario description</span></span>
<span data-ttu-id="aeeb4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aeeb4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aeeb4-121">Přidání New Relic z Galerie</span><span class="sxs-lookup"><span data-stu-id="aeeb4-121">Adding New Relic from the gallery</span></span>
2. <span data-ttu-id="aeeb4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="aeeb4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-new-relic-from-the-gallery"></a><span data-ttu-id="aeeb4-123">Přidání New Relic z Galerie</span><span class="sxs-lookup"><span data-stu-id="aeeb4-123">Adding New Relic from the gallery</span></span>
<span data-ttu-id="aeeb4-124">Při konfiguraci integrace New Relic do služby Azure AD potřebujete přidat New Relic z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-124">To configure the integration of New Relic into Azure AD, you need to add New Relic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="aeeb4-125">**Pokud chcete přidat New Relic z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="aeeb4-125">**To add New Relic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="aeeb4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="aeeb4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="aeeb4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="aeeb4-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="aeeb4-133">Do vyhledávacího pole zadejte **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-133">In the search box, type **New Relic**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. <span data-ttu-id="aeeb4-135">Na panelu výsledků vyberte **New Relic**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-135">In the results panel, select **New Relic**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="aeeb4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="aeeb4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="aeeb4-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s New Relic podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="aeeb4-138">In this section, you configure and test Azure AD single sign-on with New Relic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="aeeb4-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v New Relic je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in New Relic is to a user in Azure AD.</span></span> <span data-ttu-id="aeeb4-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-140">In other words, a link relationship between an Azure AD user and the related user in New Relic needs to be established.</span></span>

<span data-ttu-id="aeeb4-141">V New Relic, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-141">In New Relic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="aeeb4-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s New Relic, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-142">To configure and test Azure AD single sign-on with New Relic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="aeeb4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="aeeb4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aeeb4-145">**[Vytvoření zkušebního uživatele New Relic](#creating-a-new-relic-test-user)**  – Pokud chcete mít protějšek Britta Simon v New Relic, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-145">**[Creating a New Relic test user](#creating-a-new-relic-test-user)** - to have a counterpart of Britta Simon in New Relic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="aeeb4-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aeeb4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="aeeb4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="aeeb4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="aeeb4-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your New Relic application.</span></span>

<span data-ttu-id="aeeb4-150">**Ke konfiguraci Azure AD jednotné přihlašování s New Relic, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="aeeb4-150">**To configure Azure AD single sign-on with New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="aeeb4-151">Na portálu Azure na **New Relic** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-151">In the Azure portal, on the **New Relic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="aeeb4-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. <span data-ttu-id="aeeb4-155">Na **nové domény Relic a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-155">On the **New Relic Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    <span data-ttu-id="aeeb4-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.newrelic.com`</span><span class="sxs-lookup"><span data-stu-id="aeeb4-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.newrelic.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="aeeb4-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-158">The value is not real.</span></span> <span data-ttu-id="aeeb4-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="aeeb4-160">Obraťte se na [tým podpory pro nového klienta Relic](https://support.newrelic.com/) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-160">Contact [New Relic Client support team](https://support.newrelic.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="aeeb4-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. <span data-ttu-id="aeeb4-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="aeeb4-165">Na **novou konfiguraci Relic** klikněte na tlačítko **konfigurace New Relic** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-165">On the **New Relic Configuration** section, click **Configure New Relic** to open **Configure sign-on** window.</span></span> <span data-ttu-id="aeeb4-166">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="aeeb4-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. <span data-ttu-id="aeeb4-168">V okně prohlížeče jiný web, přihlaste se k vaší **New Relic** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-168">In a different web browser window, sign on to your **New Relic** company site as administrator.</span></span>

8. <span data-ttu-id="aeeb4-169">V nabídce v horní části, klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-169">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="aeeb4-170">![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797036.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="aeeb4-170">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Account Settings")</span></span>

9. <span data-ttu-id="aeeb4-171">Klikněte na tlačítko **zabezpečení a ověřování** a pak klikněte **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-171">Click the **Security and authentication** tab, and then click the **Single sign on** tab.</span></span>
   
    <span data-ttu-id="aeeb4-172">![Jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/ic797037.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="aeeb4-172">![Single Sign-On](./media/active-directory-saas-new-relic-tutorial/ic797037.png "Single Sign-On")</span></span>

10. <span data-ttu-id="aeeb4-173">Na stránce dialogové okno SAML proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-173">On the SAML dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="aeeb4-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="aeeb4-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span></span>
   
   <span data-ttu-id="aeeb4-175">a.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-175">a.</span></span> <span data-ttu-id="aeeb4-176">Klikněte na tlačítko **zvolit soubor** odeslání vašeho certifikátu stažené Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-176">Click **Choose File** to upload your downloaded Azure Active Directory certificate.</span></span>

   <span data-ttu-id="aeeb4-177">b.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-177">b.</span></span> <span data-ttu-id="aeeb4-178">V **adresu URL pro vzdálené přihlášení** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-178">In the **Remote login URL** textbox,  paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="aeeb4-179">c.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-179">c.</span></span> <span data-ttu-id="aeeb4-180">V **cílová stránka adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-180">In the **Logout landing URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

   <span data-ttu-id="aeeb4-181">d.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-181">d.</span></span> <span data-ttu-id="aeeb4-182">Klikněte na tlačítko **uložit změny provedené v**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-182">Click **Save my changes**.</span></span>

> [!TIP]
> <span data-ttu-id="aeeb4-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="aeeb4-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="aeeb4-184">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="aeeb4-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="aeeb4-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="aeeb4-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="aeeb4-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="aeeb4-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="aeeb4-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="aeeb4-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="aeeb4-190">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="aeeb4-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="aeeb4-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="aeeb4-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="aeeb4-198">a.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-198">a.</span></span> <span data-ttu-id="aeeb4-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aeeb4-200">b.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-200">b.</span></span> <span data-ttu-id="aeeb4-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="aeeb4-202">c.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-202">c.</span></span> <span data-ttu-id="aeeb4-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="aeeb4-204">d.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-204">d.</span></span> <span data-ttu-id="aeeb4-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-205">Click **Create**.</span></span>
 
### <a name="creating-a-new-relic-test-user"></a><span data-ttu-id="aeeb4-206">Vytvoření zkušebního uživatele New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-206">Creating a New Relic test user</span></span>

<span data-ttu-id="aeeb4-207">Pokud chcete povolit uživatelů Azure Active Directory k přihlášení do New Relic, musí být zřízená do New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-207">In order to enable Azure Active Directory users to log in to New Relic, they must be provisioned into New Relic.</span></span> <span data-ttu-id="aeeb4-208">V případě New Relic zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-208">In the case of New Relic, provisioning is a manual task.</span></span>

<span data-ttu-id="aeeb4-209">**Pokud chcete zřídit uživatelský účet New Relic, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="aeeb4-209">**To provision a user account to New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="aeeb4-210">Přihlaste se k vaší **New Relic** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-210">Log in to your **New Relic** company site as administrator.</span></span>

2. <span data-ttu-id="aeeb4-211">V nabídce v horní části, klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-211">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="aeeb4-212">![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797040.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="aeeb4-212">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Account Settings")</span></span>

3. <span data-ttu-id="aeeb4-213">V **účet** podokna na levé straně klikněte na tlačítko **Souhrn**a potom klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-213">In the **Account** pane on the left side, click **Summary**, and then click **Add user**.</span></span>
   
    <span data-ttu-id="aeeb4-214">![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797041.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="aeeb4-214">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Account Settings")</span></span>

4. <span data-ttu-id="aeeb4-215">Na **aktivní uživatelé** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="aeeb4-215">On the **Active users** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="aeeb4-216">![Aktivní uživatelé](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktivní uživatelé")</span><span class="sxs-lookup"><span data-stu-id="aeeb4-216">![Active Users](./media/active-directory-saas-new-relic-tutorial/ic797042.png "Active Users")</span></span>
   
    <span data-ttu-id="aeeb4-217">a.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-217">a.</span></span> <span data-ttu-id="aeeb4-218">V **e-mailu** textovému poli, zadejte e-mailovou adresu chcete zřídit platného uživatele Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-218">In the **Email** textbox, type the email address of a valid Azure Active Directory user you want to provision.</span></span>

    <span data-ttu-id="aeeb4-219">b.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-219">b.</span></span> <span data-ttu-id="aeeb4-220">Jako **Role** vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-220">As **Role** select **User**.</span></span>

    <span data-ttu-id="aeeb4-221">c.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-221">c.</span></span> <span data-ttu-id="aeeb4-222">Klikněte na tlačítko **přidejte tohoto uživatele**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-222">Click **Add this user**.</span></span>

>[!NOTE]
><span data-ttu-id="aeeb4-223">Můžete použít všechny ostatní New Relic uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované New Relic zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-223">You can use any other New Relic user account creation tools or APIs provided by New Relic to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="aeeb4-224">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="aeeb4-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="aeeb4-225">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to New Relic.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="aeeb4-227">**Pokud chcete přiřadit Britta Simon New Relic, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="aeeb4-227">**To assign Britta Simon to New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="aeeb4-228">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="aeeb4-230">V seznamu aplikací vyberte **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-230">In the applications list, select **New Relic**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. <span data-ttu-id="aeeb4-232">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="aeeb4-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-234">Click **Add** button.</span></span> <span data-ttu-id="aeeb4-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="aeeb4-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="aeeb4-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aeeb4-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="aeeb4-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="aeeb4-240">Testing single sign-on</span></span>

<span data-ttu-id="aeeb4-241">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-241">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="aeeb4-242">Když kliknete na dlaždici New Relic na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci New Relic.</span><span class="sxs-lookup"><span data-stu-id="aeeb4-242">When you click the New Relic tile in the Access Panel, you should get automatically signed-on to your New Relic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aeeb4-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="aeeb4-243">Additional resources</span></span>

* [<span data-ttu-id="aeeb4-244">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aeeb4-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aeeb4-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="aeeb4-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

