---
title: 'Kurz: Azure Active Directory integrace s Sugar CRM | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Sugar CRM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: c27aef24e859522b8001ecb747906abdca14d87a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a><span data-ttu-id="3d9dd-103">Kurz: Azure Active Directory integrace s Sugar CRM</span><span class="sxs-lookup"><span data-stu-id="3d9dd-103">Tutorial: Azure Active Directory integration with Sugar CRM</span></span>

<span data-ttu-id="3d9dd-104">V tomto kurzu zjistěte, jak integrovat Sugar CRM s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3d9dd-104">In this tutorial, you learn how to integrate Sugar CRM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3d9dd-105">Integrace s Azure AD Sugar CRM poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-105">Integrating Sugar CRM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3d9dd-106">Můžete řídit ve službě Azure AD, který má přístup k Sugar CRM</span><span class="sxs-lookup"><span data-stu-id="3d9dd-106">You can control in Azure AD who has access to Sugar CRM</span></span>
- <span data-ttu-id="3d9dd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Sugar CRM (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3d9dd-107">You can enable your users to automatically get signed-on to Sugar CRM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3d9dd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3d9dd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3d9dd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3d9dd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d9dd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3d9dd-110">Prerequisites</span></span>

<span data-ttu-id="3d9dd-111">Konfigurace integrace Azure AD s Sugar CRM, budete potřebovat následující položky:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-111">To configure Azure AD integration with Sugar CRM, you need the following items:</span></span>

- <span data-ttu-id="3d9dd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3d9dd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3d9dd-113">Sugar CRM jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3d9dd-113">A Sugar CRM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3d9dd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3d9dd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3d9dd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3d9dd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d9dd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3d9dd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3d9dd-118">Scenario description</span></span>
<span data-ttu-id="3d9dd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3d9dd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3d9dd-121">Přidání Sugar CRM z Galerie</span><span class="sxs-lookup"><span data-stu-id="3d9dd-121">Adding Sugar CRM from the gallery</span></span>
2. <span data-ttu-id="3d9dd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3d9dd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sugar-crm-from-the-gallery"></a><span data-ttu-id="3d9dd-123">Přidání Sugar CRM z Galerie</span><span class="sxs-lookup"><span data-stu-id="3d9dd-123">Adding Sugar CRM from the gallery</span></span>
<span data-ttu-id="3d9dd-124">Při konfiguraci integrace Sugar CRM do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Sugar CRM z galerie.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-124">To configure the integration of Sugar CRM into Azure AD, you need to add Sugar CRM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3d9dd-125">**Pokud chcete přidat Sugar CRM z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3d9dd-125">**To add Sugar CRM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9dd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3d9dd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3d9dd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3d9dd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3d9dd-133">Do vyhledávacího pole zadejte **Sugar CRM**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-133">In the search box, type **Sugar CRM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_search.png)

5. <span data-ttu-id="3d9dd-135">Na panelu výsledků vyberte **Sugar CRM**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-135">In the results panel, select **Sugar CRM**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3d9dd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3d9dd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3d9dd-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sugar CRM podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3d9dd-138">In this section, you configure and test Azure AD single sign-on with Sugar CRM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3d9dd-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Sugar CRM je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sugar CRM is to a user in Azure AD.</span></span> <span data-ttu-id="3d9dd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Sugar CRM je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-140">In other words, a link relationship between an Azure AD user and the related user in Sugar CRM needs to be established.</span></span>

<span data-ttu-id="3d9dd-141">V Sugar CRM přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-141">In Sugar CRM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3d9dd-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sugar CRM, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-142">To configure and test Azure AD single sign-on with Sugar CRM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3d9dd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3d9dd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3d9dd-145">**[Vytvoření zkušebního uživatele Sugar CRM](#creating-a-sugar-crm-test-user)**  – Pokud chcete mít protějšek Britta Simon v CRM Sugar, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-145">**[Creating a Sugar CRM test user](#creating-a-sugar-crm-test-user)** - to have a counterpart of Britta Simon in Sugar CRM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3d9dd-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3d9dd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3d9dd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3d9dd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3d9dd-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Sugar CRM.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sugar CRM application.</span></span>

<span data-ttu-id="3d9dd-150">**Ke konfiguraci Azure AD jednotné přihlašování s Sugar CRM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3d9dd-150">**To configure Azure AD single sign-on with Sugar CRM, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9dd-151">Na portálu Azure na **Sugar CRM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-151">In the Azure portal, on the **Sugar CRM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3d9dd-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

3. <span data-ttu-id="3d9dd-155">Na **Sugar CRM domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-155">On the **Sugar CRM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    <span data-ttu-id="3d9dd-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-157">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > <span data-ttu-id="3d9dd-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-158">The value is not real.</span></span> <span data-ttu-id="3d9dd-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="3d9dd-160">Obraťte se na [tým podpory klienta CRM Sugar](https://support.sugarcrm.com/) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-160">Contact [Sugar CRM Client support team](https://support.sugarcrm.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="3d9dd-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

5. <span data-ttu-id="3d9dd-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3d9dd-165">Na **Sugar CRM konfigurace** klikněte na tlačítko **konfigurace CRM Sugar** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-165">On the **Sugar CRM Configuration** section, click **Configure Sugar CRM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3d9dd-166">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3d9dd-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

7. <span data-ttu-id="3d9dd-168">V okně prohlížeče jiný web Přihlaste se na váš web společnosti Sugar CRM jako správce.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-168">In a different web browser window, log in to your Sugar CRM company site as an administrator.</span></span>

8. <span data-ttu-id="3d9dd-169">Přejděte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-169">Go to **Admin**.</span></span>
   
    <span data-ttu-id="3d9dd-170">![Správce](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "správce")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

9. <span data-ttu-id="3d9dd-171">V **správy** klikněte na tlačítko **Správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-171">In the **Administration** section, click **Password Management**.</span></span>
   
    <span data-ttu-id="3d9dd-172">![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "správy")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span></span>

10. <span data-ttu-id="3d9dd-173">Vyberte **povolit ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-173">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="3d9dd-174">![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "správy")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span></span>

11. <span data-ttu-id="3d9dd-175">V **ověřování SAML** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-175">In the **SAML Authentication** section, perform the following steps:</span></span>
   
    <span data-ttu-id="3d9dd-176">![Ověřování SAML](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-176">![SAML Authentication](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML Authentication")</span></span>  
 
    <span data-ttu-id="3d9dd-177">a.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-177">a.</span></span> <span data-ttu-id="3d9dd-178">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-178">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="3d9dd-179">b.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-179">b.</span></span> <span data-ttu-id="3d9dd-180">V **SLO URL** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-180">In the **SLO URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="3d9dd-181">c.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-181">c.</span></span> <span data-ttu-id="3d9dd-182">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte celý certifikát do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-182">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="3d9dd-183">d.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-183">d.</span></span> <span data-ttu-id="3d9dd-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3d9dd-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3d9dd-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3d9dd-186">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3d9dd-187">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3d9dd-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3d9dd-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3d9dd-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="3d9dd-189">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3d9dd-191">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3d9dd-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9dd-192">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3d9dd-194">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3d9dd-196">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3d9dd-198">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3d9dd-200">a.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-200">a.</span></span> <span data-ttu-id="3d9dd-201">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3d9dd-202">b.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-202">b.</span></span> <span data-ttu-id="3d9dd-203">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3d9dd-204">c.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-204">c.</span></span> <span data-ttu-id="3d9dd-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3d9dd-206">d.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-206">d.</span></span> <span data-ttu-id="3d9dd-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-207">Click **Create**.</span></span>
 
### <a name="creating-a-sugar-crm-test-user"></a><span data-ttu-id="3d9dd-208">Vytvoření zkušebního uživatele Sugar CRM</span><span class="sxs-lookup"><span data-stu-id="3d9dd-208">Creating a Sugar CRM test user</span></span>

<span data-ttu-id="3d9dd-209">Pokud chcete povolit uživatelům Azure AD přihlášení k Sugar CRM, musí být zřízená k Sugar CRM.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-209">In order to enable Azure AD users to log in to Sugar CRM, they must be provisioned to Sugar CRM.</span></span>

<span data-ttu-id="3d9dd-210">V případě Sugar CRM je zřizování ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-210">In the case of Sugar CRM, provisioning is a manual task.</span></span>

<span data-ttu-id="3d9dd-211">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3d9dd-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9dd-212">Přihlaste se k vaší **Sugar CRM** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-212">Log in to your **Sugar CRM** company site as administrator.</span></span>

2. <span data-ttu-id="3d9dd-213">Přejděte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-213">Go to **Admin**.</span></span>
   
    <span data-ttu-id="3d9dd-214">![Správce](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "správce")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

3. <span data-ttu-id="3d9dd-215">V **správy** klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-215">In the **Administration** section, click **User Management**.</span></span>
   
    <span data-ttu-id="3d9dd-216">![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "správy")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span></span>

4. <span data-ttu-id="3d9dd-217">Přejděte na **uživatelé \> vytvořit nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-217">Go to **Users \> Create New User**.</span></span>
   
    <span data-ttu-id="3d9dd-218">![Vytvoření nového uživatele](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "vytvořit nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-218">![Create New User](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Create New User")</span></span>

5. <span data-ttu-id="3d9dd-219">Na **profil uživatele** kartu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-219">On the **User Profile** tab, perform the following steps:</span></span>
   
    <span data-ttu-id="3d9dd-220">![Nový uživatel](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-220">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "New User")</span></span>

    <span data-ttu-id="3d9dd-221">a.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-221">a.</span></span> <span data-ttu-id="3d9dd-222">Typ **uživatelské jméno**, **příjmení**, a **e-mailová adresa** platný uživatele Azure Active Directory do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-222">Type the **user name**, **last name**, and **email address** of a valid Azure Active Directory user into the related textboxes.</span></span>
  
6. <span data-ttu-id="3d9dd-223">Jako **stav**, vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-223">As **Status**, select **Active**.</span></span>

7. <span data-ttu-id="3d9dd-224">Na kartě heslo proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3d9dd-224">On the Password tab, perform the following steps:</span></span>
   
    <span data-ttu-id="3d9dd-225">![Nový uživatel](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="3d9dd-225">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "New User")</span></span>

    <span data-ttu-id="3d9dd-226">a.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-226">a.</span></span> <span data-ttu-id="3d9dd-227">Zadejte heslo do textového pole související.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-227">Type the password into the related textbox.</span></span>

    <span data-ttu-id="3d9dd-228">b.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-228">b.</span></span> <span data-ttu-id="3d9dd-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-229">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="3d9dd-230">Můžete použít všechny ostatní Sugar CRM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Sugar CRM zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-230">You can use any other Sugar CRM user account creation tools or APIs provided by Sugar CRM to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3d9dd-231">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3d9dd-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3d9dd-232">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k Sugar CRM.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sugar CRM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3d9dd-234">**Pokud chcete přiřadit Britta Simon Sugar CRM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3d9dd-234">**To assign Britta Simon to Sugar CRM, perform the following steps:**</span></span>

1. <span data-ttu-id="3d9dd-235">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3d9dd-237">V seznamu aplikací vyberte **Sugar CRM**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-237">In the applications list, select **Sugar CRM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

3. <span data-ttu-id="3d9dd-239">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3d9dd-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-241">Click **Add** button.</span></span> <span data-ttu-id="3d9dd-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3d9dd-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3d9dd-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3d9dd-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3d9dd-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3d9dd-247">Testing single sign-on</span></span>

<span data-ttu-id="3d9dd-248">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-248">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3d9dd-249">Když kliknete na dlaždici Sugar CRM na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Sugar CRM.</span><span class="sxs-lookup"><span data-stu-id="3d9dd-249">When you click the Sugar CRM tile in the Access Panel, you should get automatically signed-on to your Sugar CRM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3d9dd-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3d9dd-250">Additional resources</span></span>

* [<span data-ttu-id="3d9dd-251">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3d9dd-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3d9dd-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3d9dd-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_203.png

