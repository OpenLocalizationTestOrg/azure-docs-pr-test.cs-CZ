---
title: "Kurz: Azure Active Directory integrace s EthicsPoint Incident správy (EPIM) | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a EthicsPoint Incident správy (EPIM)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: b5ac3afd973b5765ba151e766754934b49ac0e0c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a><span data-ttu-id="3aaba-103">Kurz: Azure Active Directory integrace s EthicsPoint Incident správy (EPIM)</span><span class="sxs-lookup"><span data-stu-id="3aaba-103">Tutorial: Azure Active Directory integration with EthicsPoint Incident Management (EPIM)</span></span>

<span data-ttu-id="3aaba-104">V tomto kurzu zjistěte, jak integrovat EthicsPoint Incident správy (EPIM) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3aaba-104">In this tutorial, you learn how to integrate EthicsPoint Incident Management (EPIM) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3aaba-105">Integrace EthicsPoint Incident správy (EPIM) s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3aaba-105">Integrating EthicsPoint Incident Management (EPIM) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3aaba-106">Můžete řídit ve službě Azure AD, který má přístup k EthicsPoint Incident správy (EPIM)</span><span class="sxs-lookup"><span data-stu-id="3aaba-106">You can control in Azure AD who has access to EthicsPoint Incident Management (EPIM)</span></span>
- <span data-ttu-id="3aaba-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k EthicsPoint Incident správy (EPIM) (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aaba-107">You can enable your users to automatically get signed-on to EthicsPoint Incident Management (EPIM) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3aaba-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3aaba-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3aaba-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3aaba-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3aaba-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3aaba-110">Prerequisites</span></span>

<span data-ttu-id="3aaba-111">Ke konfiguraci integrace služby Azure AD s EthicsPoint Incident správy (EPIM), potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3aaba-111">To configure Azure AD integration with EthicsPoint Incident Management (EPIM), you need the following items:</span></span>

- <span data-ttu-id="3aaba-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aaba-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3aaba-113">EthicsPoint Incident správy (EPIM) jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3aaba-113">A EthicsPoint Incident Management (EPIM) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3aaba-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3aaba-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3aaba-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3aaba-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3aaba-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3aaba-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3aaba-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3aaba-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3aaba-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3aaba-118">Scenario description</span></span>
<span data-ttu-id="3aaba-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3aaba-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3aaba-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3aaba-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3aaba-121">Přidání EthicsPoint Incident správy (EPIM) z Galerie</span><span class="sxs-lookup"><span data-stu-id="3aaba-121">Adding EthicsPoint Incident Management (EPIM) from the gallery</span></span>
2. <span data-ttu-id="3aaba-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aaba-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a><span data-ttu-id="3aaba-123">Přidání EthicsPoint Incident správy (EPIM) z Galerie</span><span class="sxs-lookup"><span data-stu-id="3aaba-123">Adding EthicsPoint Incident Management (EPIM) from the gallery</span></span>
<span data-ttu-id="3aaba-124">Chcete-li konfigurace integrace nástroje správy incidentu EthicsPoint (EPIM) do služby Azure AD, přidejte EthicsPoint Incident správy (EPIM) z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3aaba-124">To configure the integration of EthicsPoint Incident Management (EPIM) into Azure AD, you need to add EthicsPoint Incident Management (EPIM) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3aaba-125">**Pokud chcete přidat EthicsPoint Incident správy (EPIM) z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aaba-125">**To add EthicsPoint Incident Management (EPIM) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3aaba-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3aaba-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3aaba-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3aaba-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3aaba-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aaba-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3aaba-133">Do vyhledávacího pole zadejte **EthicsPoint Incident správy (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-133">In the search box, type **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

5. <span data-ttu-id="3aaba-135">Na panelu výsledků vyberte **EthicsPoint Incident správy (EPIM)**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3aaba-135">In the results panel, select **EthicsPoint Incident Management (EPIM)**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3aaba-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aaba-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3aaba-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s EthicsPoint Incident správy (EPIM) podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3aaba-138">In this section, you configure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM) based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3aaba-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v EthicsPoint Incident správy (EPIM) je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3aaba-139">For single sign-on to work, Azure AD needs to know what the counterpart user in EthicsPoint Incident Management (EPIM) is to a user in Azure AD.</span></span> <span data-ttu-id="3aaba-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v EthicsPoint Incident správy (EPIM).</span><span class="sxs-lookup"><span data-stu-id="3aaba-140">In other words, a link relationship between an Azure AD user and the related user in EthicsPoint Incident Management (EPIM) needs to be established.</span></span>

<span data-ttu-id="3aaba-141">V EthicsPoint Incident správy (EPIM), přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3aaba-141">In EthicsPoint Incident Management (EPIM), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3aaba-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s EthicsPoint Incident správy (EPIM), je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3aaba-142">To configure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3aaba-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3aaba-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3aaba-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3aaba-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3aaba-145">**[Vytvoření zkušebního uživatele EthicsPoint Incident správy (EPIM)](#creating-a-ethicspoint-incident-management-epim-test-user)**  – Pokud chcete mít protějšek Britta Simon v EthicsPoint Incident správy (EPIM), propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3aaba-145">**[Creating a EthicsPoint Incident Management (EPIM) test user](#creating-a-ethicspoint-incident-management-epim-test-user)** - to have a counterpart of Britta Simon in EthicsPoint Incident Management (EPIM) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3aaba-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3aaba-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3aaba-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3aaba-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3aaba-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aaba-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3aaba-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci EthicsPoint Incident správy (EPIM).</span><span class="sxs-lookup"><span data-stu-id="3aaba-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your EthicsPoint Incident Management (EPIM) application.</span></span>

<span data-ttu-id="3aaba-150">**Pokud chcete konfigurovat Azure AD jednotné přihlašování s EthicsPoint Incident správy (EPIM), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aaba-150">**To configure Azure AD single sign-on with EthicsPoint Incident Management (EPIM), perform the following steps:**</span></span>

1. <span data-ttu-id="3aaba-151">Na portálu Azure na **EthicsPoint Incident správy (EPIM)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-151">In the Azure portal, on the **EthicsPoint Incident Management (EPIM)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3aaba-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3aaba-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

3. <span data-ttu-id="3aaba-155">Na **EthicsPoint Incident správy (EPIM) domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3aaba-155">On the **EthicsPoint Incident Management (EPIM) Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    <span data-ttu-id="3aaba-157">a.</span><span class="sxs-lookup"><span data-stu-id="3aaba-157">a.</span></span> <span data-ttu-id="3aaba-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="3aaba-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    <span data-ttu-id="3aaba-159">b.</span><span class="sxs-lookup"><span data-stu-id="3aaba-159">b.</span></span> <span data-ttu-id="3aaba-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.navexglobal.com/adfs/services/trust`</span><span class="sxs-lookup"><span data-stu-id="3aaba-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.navexglobal.com/adfs/services/trust`</span></span>

    <span data-ttu-id="3aaba-161">c.</span><span class="sxs-lookup"><span data-stu-id="3aaba-161">c.</span></span> <span data-ttu-id="3aaba-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<servername>.navexglobal.com/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="3aaba-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<servername>.navexglobal.com/adfs/ls/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3aaba-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3aaba-163">These values are not real.</span></span> <span data-ttu-id="3aaba-164">Tyto hodnoty aktualizujte s skutečná adresa URL odpovědi, identifikátor a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="3aaba-164">Update these values with the actual Reply URL, Identifier, and Sign-On URL.</span></span> <span data-ttu-id="3aaba-165">Obraťte se na [tým podpory EthicsPoint Incident správy (EPIM) klienta](http://www.navexglobal.com/company/contact-us) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3aaba-165">Contact [EthicsPoint Incident Management (EPIM) Client support team](http://www.navexglobal.com/company/contact-us) to get these values.</span></span> 

4. <span data-ttu-id="3aaba-166">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3aaba-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

5. <span data-ttu-id="3aaba-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aaba-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="3aaba-170">Konfigurace jednotného přihlašování na **EthicsPoint Incident správy (EPIM)** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory EthicsPoint Incident správy (EPIM)](http://www.navexglobal.com/company/contact-us).</span><span class="sxs-lookup"><span data-stu-id="3aaba-170">To configure single sign-on on **EthicsPoint Incident Management (EPIM)** side, you need to send the downloaded **Metadata XML** to [EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="3aaba-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3aaba-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3aaba-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3aaba-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3aaba-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3aaba-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3aaba-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aaba-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="3aaba-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3aaba-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3aaba-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aaba-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3aaba-178">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3aaba-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3aaba-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3aaba-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aaba-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3aaba-184">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3aaba-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3aaba-186">a.</span><span class="sxs-lookup"><span data-stu-id="3aaba-186">a.</span></span> <span data-ttu-id="3aaba-187">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3aaba-188">b.</span><span class="sxs-lookup"><span data-stu-id="3aaba-188">b.</span></span> <span data-ttu-id="3aaba-189">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3aaba-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3aaba-190">c.</span><span class="sxs-lookup"><span data-stu-id="3aaba-190">c.</span></span> <span data-ttu-id="3aaba-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3aaba-192">d.</span><span class="sxs-lookup"><span data-stu-id="3aaba-192">d.</span></span> <span data-ttu-id="3aaba-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-193">Click **Create**.</span></span>
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a><span data-ttu-id="3aaba-194">Vytvoření zkušebního uživatele EthicsPoint Incident správy (EPIM)</span><span class="sxs-lookup"><span data-stu-id="3aaba-194">Creating a EthicsPoint Incident Management (EPIM) test user</span></span>

<span data-ttu-id="3aaba-195">V této části vytvoříte uživatele názvem Britta Simon v EthicsPoint Incident správy (EPIM).</span><span class="sxs-lookup"><span data-stu-id="3aaba-195">In this section, you create a user called Britta Simon in EthicsPoint Incident Management (EPIM).</span></span> <span data-ttu-id="3aaba-196">Spojte se s [tým podpory EthicsPoint Incident správy (EPIM)](http://www.navexglobal.com/company/contact-us) přidat uživatele do platformy EthicsPoint Incident správy (EPIM).</span><span class="sxs-lookup"><span data-stu-id="3aaba-196">Please work with [EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us) to add the users in the EthicsPoint Incident Management (EPIM) platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3aaba-197">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3aaba-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3aaba-198">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k EthicsPoint Incident správy (EPIM).</span><span class="sxs-lookup"><span data-stu-id="3aaba-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to EthicsPoint Incident Management (EPIM).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3aaba-200">**Pokud chcete přiřadit Britta Simon k EthicsPoint Incident správy (EPIM), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3aaba-200">**To assign Britta Simon to EthicsPoint Incident Management (EPIM), perform the following steps:**</span></span>

1. <span data-ttu-id="3aaba-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3aaba-203">V seznamu aplikací vyberte **EthicsPoint Incident správy (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-203">In the applications list, select **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

3. <span data-ttu-id="3aaba-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3aaba-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3aaba-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3aaba-207">Click **Add** button.</span></span> <span data-ttu-id="3aaba-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aaba-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3aaba-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3aaba-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3aaba-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aaba-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3aaba-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3aaba-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3aaba-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3aaba-213">Testing single sign-on</span></span>

<span data-ttu-id="3aaba-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3aaba-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="3aaba-215">Když kliknete na dlaždici EthicsPoint Incident správy (EPIM) na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci EthicsPoint Incident správy (EPIM).</span><span class="sxs-lookup"><span data-stu-id="3aaba-215">When you click the EthicsPoint Incident Management (EPIM) tile in the Access Panel, you should get automatically signed-on to your EthicsPoint Incident Management (EPIM) application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3aaba-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3aaba-216">Additional resources</span></span>

* [<span data-ttu-id="3aaba-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3aaba-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3aaba-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3aaba-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_203.png

