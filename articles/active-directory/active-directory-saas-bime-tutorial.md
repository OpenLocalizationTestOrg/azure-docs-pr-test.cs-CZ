---
title: 'Kurz: Azure Active Directory integrace s Bime | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Bime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 8f46ff1265d302ab114747b4b45227e58718166b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a><span data-ttu-id="d973c-103">Kurz: Azure Active Directory integrace s Bime</span><span class="sxs-lookup"><span data-stu-id="d973c-103">Tutorial: Azure Active Directory integration with Bime</span></span>

<span data-ttu-id="d973c-104">V tomto kurzu zjistěte, jak integrovat Bime s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d973c-104">In this tutorial, you learn how to integrate Bime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d973c-105">Integrace Bime s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d973c-105">Integrating Bime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d973c-106">Můžete řídit ve službě Azure AD, který má přístup k Bime</span><span class="sxs-lookup"><span data-stu-id="d973c-106">You can control in Azure AD who has access to Bime</span></span>
- <span data-ttu-id="d973c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Bime (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d973c-107">You can enable your users to automatically get signed-on to Bime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d973c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d973c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d973c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d973c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d973c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d973c-110">Prerequisites</span></span>

<span data-ttu-id="d973c-111">Konfigurace integrace Azure AD s Bime, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d973c-111">To configure Azure AD integration with Bime, you need the following items:</span></span>

- <span data-ttu-id="d973c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d973c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d973c-113">Bime jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d973c-113">A Bime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d973c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d973c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d973c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d973c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d973c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d973c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d973c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d973c-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d973c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d973c-118">Scenario description</span></span>
<span data-ttu-id="d973c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d973c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d973c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d973c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d973c-121">Přidání Bime z Galerie</span><span class="sxs-lookup"><span data-stu-id="d973c-121">Adding Bime from the gallery</span></span>
2. <span data-ttu-id="d973c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d973c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bime-from-the-gallery"></a><span data-ttu-id="d973c-123">Přidání Bime z Galerie</span><span class="sxs-lookup"><span data-stu-id="d973c-123">Adding Bime from the gallery</span></span>
<span data-ttu-id="d973c-124">Při konfiguraci integrace Bime do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Bime z galerie.</span><span class="sxs-lookup"><span data-stu-id="d973c-124">To configure the integration of Bime into Azure AD, you need to add Bime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d973c-125">**Pokud chcete přidat Bime z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d973c-125">**To add Bime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d973c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d973c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d973c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d973c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d973c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d973c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d973c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d973c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d973c-133">Do vyhledávacího pole zadejte **Bime**.</span><span class="sxs-lookup"><span data-stu-id="d973c-133">In the search box, type **Bime**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_search.png)

5. <span data-ttu-id="d973c-135">Na panelu výsledků vyberte **Bime**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d973c-135">In the results panel, select **Bime**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d973c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d973c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d973c-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Bime podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d973c-138">In this section, you configure and test Azure AD single sign-on with Bime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d973c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Bime je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d973c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Bime is to a user in Azure AD.</span></span> <span data-ttu-id="d973c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Bime musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d973c-140">In other words, a link relationship between an Azure AD user and the related user in Bime needs to be established.</span></span>

<span data-ttu-id="d973c-141">V Bime, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="d973c-141">In Bime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d973c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Bime, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d973c-142">To configure and test Azure AD single sign-on with Bime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d973c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d973c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d973c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d973c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d973c-145">**[Vytvoření zkušebního uživatele Bime](#creating-a-bime-test-user)**  – Pokud chcete mít protějšek Britta Simon v Bime propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d973c-145">**[Creating a Bime test user](#creating-a-bime-test-user)** - to have a counterpart of Britta Simon in Bime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d973c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d973c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d973c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d973c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d973c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d973c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d973c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Bime.</span><span class="sxs-lookup"><span data-stu-id="d973c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Bime application.</span></span>

<span data-ttu-id="d973c-150">**Ke konfiguraci Azure AD jednotné přihlašování s Bime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d973c-150">**To configure Azure AD single sign-on with Bime, perform the following steps:**</span></span>

1. <span data-ttu-id="d973c-151">Na portálu Azure na **Bime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d973c-151">In the Azure portal, on the **Bime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d973c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d973c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_samlbase.png)

3. <span data-ttu-id="d973c-155">Na **Bime domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d973c-155">On the **Bime Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_url.png)

    <span data-ttu-id="d973c-157">a.</span><span class="sxs-lookup"><span data-stu-id="d973c-157">a.</span></span> <span data-ttu-id="d973c-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="d973c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    <span data-ttu-id="d973c-159">b.</span><span class="sxs-lookup"><span data-stu-id="d973c-159">b.</span></span> <span data-ttu-id="d973c-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="d973c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d973c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d973c-161">These values are not real.</span></span> <span data-ttu-id="d973c-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d973c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d973c-163">Obraťte se na [tým podpory Bime klienta](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d973c-163">Contact [Bime Client support team](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) to get these values.</span></span> 
 
4. <span data-ttu-id="d973c-164">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnotu z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d973c-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value from the certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_certificate.png) 

5. <span data-ttu-id="d973c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d973c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d973c-168">Na **Bime konfigurace** klikněte na tlačítko **konfigurace Bime** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d973c-168">On the **Bime Configuration** section, click **Configure Bime** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d973c-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d973c-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_configure.png) 

7. <span data-ttu-id="d973c-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Bime.</span><span class="sxs-lookup"><span data-stu-id="d973c-171">In a different web browser window, log into your Bime company site as an administrator.</span></span>

8. <span data-ttu-id="d973c-172">Na panelu nástrojů klikněte na tlačítko **správce**a potom **účet**.</span><span class="sxs-lookup"><span data-stu-id="d973c-172">In the toolbar, click **Admin**, and then **Account**.</span></span>
   
    <span data-ttu-id="d973c-173">![Správce](./media/active-directory-saas-bime-tutorial/ic775558.png "správce")</span><span class="sxs-lookup"><span data-stu-id="d973c-173">![Admin](./media/active-directory-saas-bime-tutorial/ic775558.png "Admin")</span></span>

9. <span data-ttu-id="d973c-174">Na stránce konfigurace účtu proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d973c-174">On the account configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="d973c-175">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/ic775559.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d973c-175">![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/ic775559.png "Configure Single Sign-On")</span></span>
   
    <span data-ttu-id="d973c-176">a.</span><span class="sxs-lookup"><span data-stu-id="d973c-176">a.</span></span> <span data-ttu-id="d973c-177">Vyberte **ověřování povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="d973c-177">Select **Enable SAML authentication**.</span></span>

    <span data-ttu-id="d973c-178">b.</span><span class="sxs-lookup"><span data-stu-id="d973c-178">b.</span></span> <span data-ttu-id="d973c-179">V **vzdálené adresy URL pro přihlášení** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d973c-179">In the **Remote Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d973c-180">c.</span><span class="sxs-lookup"><span data-stu-id="d973c-180">c.</span></span>  <span data-ttu-id="d973c-181">Vložení **kryptografický otisk** hodnotu z portálu Azure do **otisků prstů certifikátů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d973c-181">Paste the **Thumbprint** value from Azure portal into the **Certificate Fingerprint** textbox.</span></span>       
   
    <span data-ttu-id="d973c-182">d.</span><span class="sxs-lookup"><span data-stu-id="d973c-182">d.</span></span> <span data-ttu-id="d973c-183">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d973c-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d973c-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d973c-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d973c-185">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d973c-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d973c-186">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d973c-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d973c-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d973c-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="d973c-188">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d973c-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d973c-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d973c-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d973c-191">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d973c-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d973c-193">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d973c-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d973c-195">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d973c-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d973c-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d973c-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d973c-199">a.</span><span class="sxs-lookup"><span data-stu-id="d973c-199">a.</span></span> <span data-ttu-id="d973c-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d973c-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d973c-201">b.</span><span class="sxs-lookup"><span data-stu-id="d973c-201">b.</span></span> <span data-ttu-id="d973c-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d973c-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d973c-203">c.</span><span class="sxs-lookup"><span data-stu-id="d973c-203">c.</span></span> <span data-ttu-id="d973c-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d973c-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d973c-205">d.</span><span class="sxs-lookup"><span data-stu-id="d973c-205">d.</span></span> <span data-ttu-id="d973c-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d973c-206">Click **Create**.</span></span>
 
### <a name="creating-a-bime-test-user"></a><span data-ttu-id="d973c-207">Vytvoření zkušebního uživatele Bime</span><span class="sxs-lookup"><span data-stu-id="d973c-207">Creating a Bime test user</span></span>

<span data-ttu-id="d973c-208">Pokud chcete povolit uživatelům Azure AD přihlášení k Bime, musí být zřízená do Bime.</span><span class="sxs-lookup"><span data-stu-id="d973c-208">In order to enable Azure AD users to log in to Bime, they must be provisioned into Bime.</span></span> <span data-ttu-id="d973c-209">V případě Bime zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d973c-209">In the case of Bime, provisioning is a manual task.</span></span>

<span data-ttu-id="d973c-210">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d973c-210">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="d973c-211">Přihlaste se k vaší **Bime** klienta.</span><span class="sxs-lookup"><span data-stu-id="d973c-211">Log in to your **Bime** tenant.</span></span>

2. <span data-ttu-id="d973c-212">Na panelu nástrojů klikněte na tlačítko **správce**a potom **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d973c-212">In the toolbar, click **Admin**, and then **Users**.</span></span>
   
    <span data-ttu-id="d973c-213">![Správce](./media/active-directory-saas-bime-tutorial/ic775561.png "správce")</span><span class="sxs-lookup"><span data-stu-id="d973c-213">![Admin](./media/active-directory-saas-bime-tutorial/ic775561.png "Admin")</span></span>

3. <span data-ttu-id="d973c-214">V **seznamu uživatelů**, klikněte na tlačítko **přidat nové uživatele** ("+").</span><span class="sxs-lookup"><span data-stu-id="d973c-214">In the **Users List**, click **Add New User** (“+”).</span></span>
   
    <span data-ttu-id="d973c-215">![Uživatelé](./media/active-directory-saas-bime-tutorial/ic775562.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="d973c-215">![Users](./media/active-directory-saas-bime-tutorial/ic775562.png "Users")</span></span>

4. <span data-ttu-id="d973c-216">Na **podrobné informace o uživateli** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d973c-216">On the **User Details** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="d973c-217">![Podrobné informace o uživateli](./media/active-directory-saas-bime-tutorial/ic775563.png "podrobné informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="d973c-217">![User Details](./media/active-directory-saas-bime-tutorial/ic775563.png "User Details")</span></span>
   
    <span data-ttu-id="d973c-218">a.</span><span class="sxs-lookup"><span data-stu-id="d973c-218">a.</span></span> <span data-ttu-id="d973c-219">V **křestní jméno** textovému poli, zadejte jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d973c-219">In the **First name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="d973c-220">b.</span><span class="sxs-lookup"><span data-stu-id="d973c-220">b.</span></span> <span data-ttu-id="d973c-221">V **příjmení** textovému poli, zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d973c-221">In the **Last name** textbox, enter the last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="d973c-222">c.</span><span class="sxs-lookup"><span data-stu-id="d973c-222">c.</span></span> <span data-ttu-id="d973c-223">V **e-mailu** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d973c-223">In the **Email** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="d973c-224">d.</span><span class="sxs-lookup"><span data-stu-id="d973c-224">d.</span></span> <span data-ttu-id="d973c-225">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d973c-225">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="d973c-226">Můžete použít všechny ostatní Bime uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Bime zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d973c-226">You can use any other Bime user account creation tools or APIs provided by Bime to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d973c-227">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d973c-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d973c-228">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Bime.</span><span class="sxs-lookup"><span data-stu-id="d973c-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Bime.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d973c-230">**Pokud chcete přiřadit Britta Simon Bime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d973c-230">**To assign Britta Simon to Bime, perform the following steps:**</span></span>

1. <span data-ttu-id="d973c-231">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d973c-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d973c-233">V seznamu aplikací vyberte **Bime**.</span><span class="sxs-lookup"><span data-stu-id="d973c-233">In the applications list, select **Bime**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_app.png) 

3. <span data-ttu-id="d973c-235">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d973c-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d973c-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d973c-237">Click **Add** button.</span></span> <span data-ttu-id="d973c-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d973c-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d973c-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d973c-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d973c-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d973c-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d973c-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d973c-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d973c-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d973c-243">Testing single sign-on</span></span>

<span data-ttu-id="d973c-244">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d973c-244">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d973c-245">Když kliknete na dlaždici Bime na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Bime.</span><span class="sxs-lookup"><span data-stu-id="d973c-245">When you click the Bime tile in the Access Panel, you should get automatically signed-on to your Bime application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d973c-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d973c-246">Additional resources</span></span>

* [<span data-ttu-id="d973c-247">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d973c-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d973c-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d973c-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bime-tutorial/tutorial_general_203.png

