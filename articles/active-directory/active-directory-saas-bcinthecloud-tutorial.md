---
title: 'Kurz: Azure Active Directory integrace s BC v cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BC v cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7dc40d2c-6349-40cb-b304-b098bd03a66c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/1/2017
ms.author: jeedes
ms.openlocfilehash: ebc95d600eca1027331cd92cfe481d0c3ee833a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bc-in-the-cloud"></a><span data-ttu-id="7f965-103">Kurz: Azure Active Directory integrace s BC v cloudu</span><span class="sxs-lookup"><span data-stu-id="7f965-103">Tutorial: Azure Active Directory integration with BC in the Cloud</span></span>

<span data-ttu-id="7f965-104">V tomto kurzu zjistěte, jak integrovat BC v cloudu pomocí Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7f965-104">In this tutorial, you learn how to integrate BC in the Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7f965-105">Integrace BC v cloudu s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7f965-105">Integrating BC in the Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7f965-106">Můžete řídit ve službě Azure AD, který má přístup k BC v cloudu</span><span class="sxs-lookup"><span data-stu-id="7f965-106">You can control in Azure AD who has access to BC in the Cloud</span></span>
- <span data-ttu-id="7f965-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BC v cloudu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f965-107">You can enable your users to automatically get signed-on to BC in the Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7f965-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7f965-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7f965-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7f965-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f965-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f965-110">Prerequisites</span></span>

<span data-ttu-id="7f965-111">Konfigurace integrace Azure AD s BC v cloudu, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="7f965-111">To configure Azure AD integration with BC in the Cloud, you need the following items:</span></span>

- <span data-ttu-id="7f965-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f965-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7f965-113">BC v v cloudu jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7f965-113">A BC in the Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7f965-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f965-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7f965-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7f965-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7f965-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7f965-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7f965-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7f965-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7f965-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7f965-118">Scenario description</span></span>
<span data-ttu-id="7f965-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f965-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7f965-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7f965-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7f965-121">Přidání BC v cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="7f965-121">Adding BC in the Cloud from the gallery</span></span>
2. <span data-ttu-id="7f965-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f965-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bc-in-the-cloud-from-the-gallery"></a><span data-ttu-id="7f965-123">Přidání BC v cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="7f965-123">Adding BC in the Cloud from the gallery</span></span>
<span data-ttu-id="7f965-124">Konfigurace integrace BC v cloudu do služby Azure AD, potřebujete přidat BC v cloudu z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7f965-124">To configure the integration of BC in the Cloud into Azure AD, you need to add BC in the Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7f965-125">**Pokud chcete přidat BC v cloudu z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7f965-125">**To add BC in the Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7f965-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f965-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7f965-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7f965-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7f965-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f965-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7f965-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f965-131">To add new application, click **Add** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7f965-133">Do vyhledávacího pole zadejte **BC v cloudu**.</span><span class="sxs-lookup"><span data-stu-id="7f965-133">In the search box, type **BC in the Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_search.png)

5. <span data-ttu-id="7f965-135">Na panelu výsledků vyberte **BC v cloudu**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7f965-135">In the results panel, select **BC in the Cloud**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7f965-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f965-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7f965-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BC v cloudu podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="7f965-138">In this section, you configure and test Azure AD single sign-on with BC in the Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7f965-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BC v cloudu je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f965-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BC in the Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="7f965-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v BC v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7f965-140">In other words, a link relationship between an Azure AD user and the related user in BC in the Cloud needs to be established.</span></span>

<span data-ttu-id="7f965-141">V BC v cloudu, přiřaďte hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="7f965-141">In BC in the Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7f965-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BC v cloudu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="7f965-142">To configure and test Azure AD single sign-on with BC in the Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7f965-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="7f965-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7f965-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7f965-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7f965-145">**[Vytváření BC v cloudu testovacího uživatele](#creating-a-bc-in-the-cloud-test-user)**  – Pokud chcete mít protějšek Britta Simon v BC v cloudu, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7f965-145">**[Creating a BC in the Cloud test user](#creating-a-bc-in-the-cloud-test-user)** - to have a counterpart of Britta Simon in BC in the Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7f965-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f965-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7f965-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7f965-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7f965-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f965-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7f965-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší BC v cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f965-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BC in the Cloud application.</span></span>

<span data-ttu-id="7f965-150">**Ke konfiguraci Azure AD jednotné přihlašování s BC v cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7f965-150">**To configure Azure AD single sign-on with BC in the Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7f965-151">Na portálu Azure na **BC v cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7f965-151">In the Azure portal, on the **BC in the Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7f965-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f965-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_samlbase.png)

3. <span data-ttu-id="7f965-155">Na **BC v cloudu domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f965-155">On the **BC in the Cloud Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_url.png)

    <span data-ttu-id="7f965-157">a.</span><span class="sxs-lookup"><span data-stu-id="7f965-157">a.</span></span> <span data-ttu-id="7f965-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span><span class="sxs-lookup"><span data-stu-id="7f965-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span></span>

    <span data-ttu-id="7f965-159">b.</span><span class="sxs-lookup"><span data-stu-id="7f965-159">b.</span></span> <span data-ttu-id="7f965-160">V **identifikátor** textovému poli, zadejte adresu URL jako:`https://app.bcinthecloud.com`</span><span class="sxs-lookup"><span data-stu-id="7f965-160">In the **Identifier** textbox, type a URL as: `https://app.bcinthecloud.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7f965-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="7f965-161">This value is not real.</span></span> <span data-ttu-id="7f965-162">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f965-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="7f965-163">Obraťte se na [tým podpory BC v klientovi cloudu](https://www.bcinthecloud.com/supportcenter/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7f965-163">Contact [BC in the Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) to get this value.</span></span> 
 
4. <span data-ttu-id="7f965-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7f965-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_certificate.png) 

5. <span data-ttu-id="7f965-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f965-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7f965-168">Konfigurace jednotného přihlašování na **BC v cloudu** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory BC v cloudu](https://www.bcinthecloud.com/supportcenter/).</span><span class="sxs-lookup"><span data-stu-id="7f965-168">To configure single sign-on on **BC in the Cloud** side, you need to send the downloaded **Metadata XML** to [BC in the Cloud support team](https://www.bcinthecloud.com/supportcenter/).</span></span>

> [!TIP]
> <span data-ttu-id="7f965-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="7f965-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7f965-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="7f965-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7f965-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7f965-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7f965-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f965-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="7f965-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7f965-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7f965-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7f965-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7f965-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f965-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7f965-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7f965-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7f965-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f965-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7f965-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f965-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7f965-184">a.</span><span class="sxs-lookup"><span data-stu-id="7f965-184">a.</span></span> <span data-ttu-id="7f965-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7f965-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7f965-186">b.</span><span class="sxs-lookup"><span data-stu-id="7f965-186">b.</span></span> <span data-ttu-id="7f965-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7f965-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7f965-188">c.</span><span class="sxs-lookup"><span data-stu-id="7f965-188">c.</span></span> <span data-ttu-id="7f965-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7f965-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7f965-190">d.</span><span class="sxs-lookup"><span data-stu-id="7f965-190">d.</span></span> <span data-ttu-id="7f965-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7f965-191">Click **Create**.</span></span>
 
### <a name="creating-a-bc-in-the-cloud-test-user"></a><span data-ttu-id="7f965-192">Vytváření BC v cloudu testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7f965-192">Creating a BC in the Cloud test user</span></span>

<span data-ttu-id="7f965-193">V této části vytvoříte uživatele volal Britta Simon v BC v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7f965-193">In this section, you create a user called Britta Simon in BC in the Cloud.</span></span> <span data-ttu-id="7f965-194">Práce s [tým podpory BC v klientovi cloudu](https://www.bcinthecloud.com/supportcenter/) přidat uživatele do BC v cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f965-194">Work with [BC in the Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) to add the users in the BC in the Cloud application.</span></span> <span data-ttu-id="7f965-195">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f965-195">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7f965-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f965-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7f965-197">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BC v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7f965-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BC in the Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7f965-199">**Pokud chcete přiřadit Britta Simon BC v cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7f965-199">**To assign Britta Simon to BC in the Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7f965-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f965-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7f965-202">V seznamu aplikací vyberte **BC v cloudu**.</span><span class="sxs-lookup"><span data-stu-id="7f965-202">In the applications list, select **BC in the Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_app.png) 

3. <span data-ttu-id="7f965-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7f965-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7f965-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f965-206">Click **Add** button.</span></span> <span data-ttu-id="7f965-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f965-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7f965-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7f965-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7f965-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f965-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7f965-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f965-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7f965-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f965-212">Testing single sign-on</span></span>

<span data-ttu-id="7f965-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7f965-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

 <span data-ttu-id="7f965-214">Po kliknutí na tlačítko BC na dlaždici cloudu na přístupovém panelu, jste měli získat automaticky přihlášení k vaší BC v cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="7f965-214">When you click the BC in the Cloud tile in the Access Panel, you should get automatically signed-on to your BC in the Cloud application.</span></span> <span data-ttu-id="7f965-215">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7f965-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f965-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7f965-216">Additional resources</span></span>

* [<span data-ttu-id="7f965-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7f965-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7f965-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7f965-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_203.png

