---
title: "Kurz: Azure Active Directory integrace s Proofpoint na vyžádání | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Proofpoint na vyžádání."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: b4c8d8c187fc865a905016f04a41843894249f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a><span data-ttu-id="d8030-103">Kurz: Azure Active Directory integrace s Proofpoint na vyžádání</span><span class="sxs-lookup"><span data-stu-id="d8030-103">Tutorial: Azure Active Directory integration with Proofpoint on Demand</span></span>

<span data-ttu-id="d8030-104">V tomto kurzu zjistěte, jak integrovat Proofpoint na vyžádání s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d8030-104">In this tutorial, you learn how to integrate Proofpoint on Demand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d8030-105">Integrace Proofpoint na vyžádání s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d8030-105">Integrating Proofpoint on Demand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d8030-106">Můžete řídit ve službě Azure AD, který má přístup k Proofpoint na vyžádání</span><span class="sxs-lookup"><span data-stu-id="d8030-106">You can control in Azure AD who has access to Proofpoint on Demand</span></span>
- <span data-ttu-id="d8030-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Proofpoint na vyžádání (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8030-107">You can enable your users to automatically get signed-on to Proofpoint on Demand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d8030-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d8030-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d8030-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d8030-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8030-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d8030-110">Prerequisites</span></span>

<span data-ttu-id="d8030-111">Konfigurace integrace Azure AD s Proofpoint na vyžádání, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d8030-111">To configure Azure AD integration with Proofpoint on Demand, you need the following items:</span></span>

- <span data-ttu-id="d8030-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8030-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d8030-113">Proofpoint na vyžádání jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d8030-113">A Proofpoint on Demand single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d8030-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8030-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d8030-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d8030-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d8030-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d8030-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d8030-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8030-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d8030-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d8030-118">Scenario description</span></span>
<span data-ttu-id="d8030-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d8030-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d8030-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d8030-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d8030-121">Přidání Proofpoint na vyžádání z Galerie</span><span class="sxs-lookup"><span data-stu-id="d8030-121">Adding Proofpoint on Demand from the gallery</span></span>
2. <span data-ttu-id="d8030-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d8030-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-proofpoint-on-demand-from-the-gallery"></a><span data-ttu-id="d8030-123">Přidání Proofpoint na vyžádání z Galerie</span><span class="sxs-lookup"><span data-stu-id="d8030-123">Adding Proofpoint on Demand from the gallery</span></span>
<span data-ttu-id="d8030-124">Chcete-li nakonfigurovat integraci Proofpoint na vyžádání do služby Azure AD, přidejte Proofpoint na vyžádání z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d8030-124">To configure the integration of Proofpoint on Demand into Azure AD, you need to add Proofpoint on Demand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d8030-125">**Pokud chcete přidat Proofpoint na vyžádání z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d8030-125">**To add Proofpoint on Demand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d8030-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d8030-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d8030-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d8030-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d8030-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d8030-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d8030-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d8030-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d8030-133">Do vyhledávacího pole zadejte **Proofpoint na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="d8030-133">In the search box, type **Proofpoint on Demand**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_search.png)

5. <span data-ttu-id="d8030-135">Na panelu výsledků vyberte **Proofpoint na vyžádání**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d8030-135">In the results panel, select **Proofpoint on Demand**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d8030-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d8030-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d8030-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Proofpoint na vyžádání v testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="d8030-138">In this section, you configure and test Azure AD single sign-on with Proofpoint on Demand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d8030-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Proofpoint na vyžádání je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8030-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Proofpoint on Demand is to a user in Azure AD.</span></span> <span data-ttu-id="d8030-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Proofpoint na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="d8030-140">In other words, a link relationship between an Azure AD user and the related user in Proofpoint on Demand needs to be established.</span></span>

<span data-ttu-id="d8030-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Proofpoint na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="d8030-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Proofpoint on Demand.</span></span>

<span data-ttu-id="d8030-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Proofpoint na vyžádání, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d8030-142">To configure and test Azure AD single sign-on with Proofpoint on Demand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d8030-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d8030-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d8030-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8030-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d8030-145">**[Vytváření Proofpoint na vyžádání testovacího uživatele](#creating-a-proofpoint-on-demand-test-user)**  – Pokud chcete mít protějšek Britta Simon v Proofpoint na vyžádání, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d8030-145">**[Creating a Proofpoint on Demand test user](#creating-a-proofpoint-on-demand-test-user)** - to have a counterpart of Britta Simon in Proofpoint on Demand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d8030-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d8030-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d8030-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d8030-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d8030-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d8030-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d8030-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší Proofpoint na vyžádání aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8030-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Proofpoint on Demand application.</span></span>

<span data-ttu-id="d8030-150">**Ke konfiguraci Azure AD jednotné přihlašování s Proofpoint na vyžádání, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d8030-150">**To configure Azure AD single sign-on with Proofpoint on Demand, perform the following steps:**</span></span>

1. <span data-ttu-id="d8030-151">Na portálu Azure na **Proofpoint na vyžádání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d8030-151">In the Azure portal, on the **Proofpoint on Demand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d8030-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d8030-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. <span data-ttu-id="d8030-155">Na **Proofpoint na vyžádání domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d8030-155">On the **Proofpoint on Demand Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    <span data-ttu-id="d8030-157">a.In **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<hostname>.pphosted.com/ppssamlsp_hostname`</span><span class="sxs-lookup"><span data-stu-id="d8030-157">a.In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com/ppssamlsp_hostname`</span></span>

    <span data-ttu-id="d8030-158">b.</span><span class="sxs-lookup"><span data-stu-id="d8030-158">b.</span></span> <span data-ttu-id="d8030-159">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<hostname>.pphosted.com/ppssamlsp`</span><span class="sxs-lookup"><span data-stu-id="d8030-159">In the **Identifier** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com/ppssamlsp`</span></span>

    <span data-ttu-id="d8030-160">c.</span><span class="sxs-lookup"><span data-stu-id="d8030-160">c.</span></span>  <span data-ttu-id="d8030-161">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span><span class="sxs-lookup"><span data-stu-id="d8030-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="d8030-162">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="d8030-162">These values are not the real.</span></span> <span data-ttu-id="d8030-163">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="d8030-163">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="d8030-164">Obraťte se na [Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d8030-164">Contact [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) to get these values.</span></span> 

4. <span data-ttu-id="d8030-165">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="d8030-165">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

5. <span data-ttu-id="d8030-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d8030-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="d8030-169">Na **Proofpoint konfiguraci na vyžádání** klikněte na tlačítko **Proofpoint konfiguraci na vyžádání** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d8030-169">On the **Proofpoint on Demand Configuration** section, click **Configure Proofpoint on Demand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d8030-170">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d8030-170">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

7. <span data-ttu-id="d8030-172">Konfigurace jednotného přihlašování na **Proofpoint na vyžádání** straně, budete muset odeslat stažené **Certificate(Base64)**,**SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** k [Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services).</span><span class="sxs-lookup"><span data-stu-id="d8030-172">To configure single sign-on on **Proofpoint on Demand** side, you need to send the downloaded **Certificate(Base64)**,**SAML Entity ID**, and **SAML Single Sign-On Service URL** to [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services).</span></span>

> [!TIP]
> <span data-ttu-id="d8030-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d8030-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d8030-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d8030-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d8030-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d8030-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d8030-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8030-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="d8030-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8030-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d8030-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d8030-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d8030-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d8030-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d8030-182">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="d8030-182">These values are not the real.</span></span> <span data-ttu-id="d8030-183">Tyto hodnoty aktualizovat se skutečnou</span><span class="sxs-lookup"><span data-stu-id="d8030-183">Update these values with the actual</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d8030-185">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d8030-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d8030-187">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d8030-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d8030-189">a.</span><span class="sxs-lookup"><span data-stu-id="d8030-189">a.</span></span> <span data-ttu-id="d8030-190">V **název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d8030-190">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="d8030-191">b.</span><span class="sxs-lookup"><span data-stu-id="d8030-191">b.</span></span> <span data-ttu-id="d8030-192">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8030-192">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="d8030-193">c.</span><span class="sxs-lookup"><span data-stu-id="d8030-193">c.</span></span> <span data-ttu-id="d8030-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d8030-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d8030-195">d.</span><span class="sxs-lookup"><span data-stu-id="d8030-195">d.</span></span> <span data-ttu-id="d8030-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d8030-196">Click **Create**.</span></span>
 
### <a name="creating-a-proofpoint-on-demand-test-user"></a><span data-ttu-id="d8030-197">Vytváření Proofpoint na vyžádání testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d8030-197">Creating a Proofpoint on Demand test user</span></span>

<span data-ttu-id="d8030-198">V této části vytvoříte uživatele volal Britta Simon v Proofpoint na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="d8030-198">In this section, you create a user called Britta Simon in Proofpoint on Demand.</span></span> <span data-ttu-id="d8030-199">Práce s [Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services) pro přidání uživatelů Proofpoint na vyžádání platformě.</span><span class="sxs-lookup"><span data-stu-id="d8030-199">Work with [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) to add users in the Proofpoint on Demand platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d8030-200">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8030-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d8030-201">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Proofpoint na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="d8030-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Proofpoint on Demand.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d8030-203">**Pokud chcete přiřadit Britta Simon Proofpoint na vyžádání, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d8030-203">**To assign Britta Simon to Proofpoint on Demand, perform the following steps:**</span></span>

1. <span data-ttu-id="d8030-204">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d8030-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d8030-206">V seznamu aplikací vyberte **Proofpoint na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="d8030-206">In the applications list, select **Proofpoint on Demand**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png) 

3. <span data-ttu-id="d8030-208">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d8030-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d8030-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d8030-210">Click **Add** button.</span></span> <span data-ttu-id="d8030-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d8030-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d8030-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d8030-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d8030-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d8030-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d8030-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d8030-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d8030-216">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d8030-216">Testing single sign-on</span></span>

<span data-ttu-id="d8030-217">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d8030-217">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d8030-218">Když kliknete **Proofpoint na vyžádání** dlaždici na přístupový Panel je by měl být automaticky přihlášeni do vaší Proofpoint na vyžádání aplikace.</span><span class="sxs-lookup"><span data-stu-id="d8030-218">When you click the **Proofpoint on Demand** tile on the Access Panel, you should be automatically signed on to your Proofpoint on Demand application.</span></span>
<span data-ttu-id="d8030-219">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d8030-219">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>  

## <a name="additional-resources"></a><span data-ttu-id="d8030-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d8030-220">Additional resources</span></span>

* [<span data-ttu-id="d8030-221">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d8030-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8030-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d8030-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

