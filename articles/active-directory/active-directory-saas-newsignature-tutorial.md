---
title: "Kurz: Integrace Azure Active Directory pomocí portálu správy cloudu Microsoft Azure | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a portálu pro správu cloudu Microsoft Azure."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: bae5f05a161b2730bf662bcb47f20ab3e1799951
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a><span data-ttu-id="f8f3c-103">Kurz: Integrace Azure Active Directory pomocí portálu správy cloudu Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f8f3c-103">Tutorial: Azure Active Directory integration with Cloud Management Portal for Microsoft Azure</span></span>

<span data-ttu-id="f8f3c-104">V tomto kurzu zjistěte, jak integrovat portál pro správu cloudu pro Microsoft Azure s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f8f3c-104">In this tutorial, you learn how to integrate Cloud Management Portal for Microsoft Azure with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f8f3c-105">Integrace portálu pro správu cloudu Microsoft Azure s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-105">Integrating Cloud Management Portal for Microsoft Azure with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f8f3c-106">Můžete řídit ve službě Azure AD, kdo má přístup k portálu pro správu cloudu pro Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f8f3c-106">You can control in Azure AD who has access to Cloud Management Portal for Microsoft Azure</span></span>
- <span data-ttu-id="f8f3c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k portálu pro správu cloudu Microsoft Azure (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8f3c-107">You can enable your users to automatically get signed-on to Cloud Management Portal for Microsoft Azure (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f8f3c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f8f3c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f8f3c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f8f3c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8f3c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f8f3c-110">Prerequisites</span></span>

<span data-ttu-id="f8f3c-111">Ke konfiguraci integrace služby Azure AD pomocí portálu správy cloudu Microsoft Azure, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-111">To configure Azure AD integration with Cloud Management Portal for Microsoft Azure, you need the following items:</span></span>

- <span data-ttu-id="f8f3c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8f3c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f8f3c-113">Portál pro správu cloudu pro Microsoft Azure jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f8f3c-113">A Cloud Management Portal for Microsoft Azure single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f8f3c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f8f3c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f8f3c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f8f3c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f8f3c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f8f3c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f8f3c-118">Scenario description</span></span>
<span data-ttu-id="f8f3c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f8f3c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f8f3c-121">Přidání portálu pro správu cloudu Microsoft Azure z Galerie</span><span class="sxs-lookup"><span data-stu-id="f8f3c-121">Adding Cloud Management Portal for Microsoft Azure from the gallery</span></span>
2. <span data-ttu-id="f8f3c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8f3c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a><span data-ttu-id="f8f3c-123">Přidání portálu pro správu cloudu Microsoft Azure z Galerie</span><span class="sxs-lookup"><span data-stu-id="f8f3c-123">Adding Cloud Management Portal for Microsoft Azure from the gallery</span></span>
<span data-ttu-id="f8f3c-124">Při konfiguraci integrace portálu pro správu cloudu Microsoft Azure do Azure AD, potřebujete přidat portál pro správu cloudu pro Microsoft Azure z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-124">To configure the integration of Cloud Management Portal for Microsoft Azure into Azure AD, you need to add Cloud Management Portal for Microsoft Azure from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f8f3c-125">**Chcete-li přidat portál pro správu cloudu pro Microsoft Azure z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f8f3c-125">**To add Cloud Management Portal for Microsoft Azure from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f8f3c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f8f3c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f8f3c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f8f3c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f8f3c-133">Do vyhledávacího pole zadejte **portál pro správu cloudu pro Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-133">In the search box, type **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_search.png)

5. <span data-ttu-id="f8f3c-135">Na panelu výsledků vyberte **portál pro správu cloudu pro Microsoft Azure**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-135">In the results panel, select **Cloud Management Portal for Microsoft Azure**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f8f3c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8f3c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f8f3c-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí portálu správy cloudu Microsoft Azure na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f8f3c-138">In this section, you configure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f8f3c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v portálu pro správu cloudu Microsoft Azure je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cloud Management Portal for Microsoft Azure is to a user in Azure AD.</span></span> <span data-ttu-id="f8f3c-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské na portálu pro správu cloudu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-140">In other words, a link relationship between an Azure AD user and the related user in Cloud Management Portal for Microsoft Azure needs to be established.</span></span>

<span data-ttu-id="f8f3c-141">V portálu pro správu cloudu Microsoft Azure, přiřaďte hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-141">In Cloud Management Portal for Microsoft Azure, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f8f3c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí portálu správy cloudu Microsoft Azure, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-142">To configure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f8f3c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f8f3c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f8f3c-145">**[Vytváření portál pro správu cloudu pro Microsoft Azure testovacího uživatele](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  – Pokud chcete mít protějšek Britta Simon v portálu pro správu cloudu Microsoft Azure, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-145">**[Creating a Cloud Management Portal for Microsoft Azure test user](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)** - to have a counterpart of Britta Simon in Cloud Management Portal for Microsoft Azure that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f8f3c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f8f3c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f8f3c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8f3c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f8f3c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v portálu pro správu cloudu pro aplikaci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="f8f3c-150">**Ke konfiguraci Azure AD jednotné přihlašování pomocí portálu správy cloudu Microsoft Azure, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f8f3c-150">**To configure Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, perform the following steps:**</span></span>

1. <span data-ttu-id="f8f3c-151">Na portálu Azure na **portál pro správu cloudu pro Microsoft Azure** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-151">In the Azure portal, on the **Cloud Management Portal for Microsoft Azure** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f8f3c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. <span data-ttu-id="f8f3c-155">Na **domény Microsoft Azure a adresy URL portálu pro správu cloudu** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-155">On the **Cloud Management Portal for Microsoft Azure Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_url.png)

    <span data-ttu-id="f8f3c-157">a.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-157">a.</span></span> <span data-ttu-id="f8f3c-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    <span data-ttu-id="f8f3c-159">b.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-159">b.</span></span> <span data-ttu-id="f8f3c-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    <span data-ttu-id="f8f3c-161">c.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-161">c.</span></span> <span data-ttu-id="f8f3c-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-162">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="f8f3c-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-163">These values are not real.</span></span> <span data-ttu-id="f8f3c-164">Tyto hodnoty aktualizujte skutečná adresa URL přihlašování, identifikátor a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-164">Update these values with the actual Sign-On URL, Identifier and Reply URL.</span></span> <span data-ttu-id="f8f3c-165">Obraťte se na [portál pro správu cloudu pro klienta Microsoft Azure tým podpory](mailto:jczernuszka@newsignature.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-165">Contact [Cloud Management Portal for Microsoft Azure Client support team](mailto:jczernuszka@newsignature.com) to get these values.</span></span> 
 
4. <span data-ttu-id="f8f3c-166">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. <span data-ttu-id="f8f3c-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f8f3c-170">Na **portál pro správu cloudu pro Microsoft Azure Configuration** klikněte na tlačítko **nakonfigurovat portál správy cloudu pro Microsoft Azure** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-170">On the **Cloud Management Portal for Microsoft Azure Configuration** section, click **Configure Cloud Management Portal for Microsoft Azure** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f8f3c-171">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f8f3c-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_configure.png) 

7. <span data-ttu-id="f8f3c-173">Konfigurace jednotného přihlašování na **portál pro správu cloudu pro Microsoft Azure** straně, budete muset odeslat stažené **certifikát**, **Sign-Out URL**, **SAML jeden přihlašování adresa URL služby** a **SAML Entity ID** k [portál pro správu cloudu pro tým podpory Microsoft Azure](mailto:jczernuszka@newsignature.com).</span><span class="sxs-lookup"><span data-stu-id="f8f3c-173">To configure single sign-on on **Cloud Management Portal for Microsoft Azure** side, you need to send the downloaded **Certificate**, **Sign-Out URL**, **SAML Single Sign-On Service URL** and **SAML Entity ID** to [Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com).</span></span> <span data-ttu-id="f8f3c-174">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-174">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f8f3c-175">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="f8f3c-175">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f8f3c-176">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-176">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f8f3c-177">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f8f3c-177">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f8f3c-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8f3c-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="f8f3c-179">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-179">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f8f3c-181">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f8f3c-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f8f3c-182">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-182">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f8f3c-184">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-184">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f8f3c-186">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-186">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f8f3c-188">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f8f3c-188">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f8f3c-190">a.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-190">a.</span></span> <span data-ttu-id="f8f3c-191">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-191">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f8f3c-192">b.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-192">b.</span></span> <span data-ttu-id="f8f3c-193">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-193">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f8f3c-194">c.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-194">c.</span></span> <span data-ttu-id="f8f3c-195">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-195">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f8f3c-196">d.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-196">d.</span></span> <span data-ttu-id="f8f3c-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-197">Click **Create**.</span></span>
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a><span data-ttu-id="f8f3c-198">Vytváření portál pro správu cloudu pro Microsoft Azure testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f8f3c-198">Creating a Cloud Management Portal for Microsoft Azure test user</span></span>

<span data-ttu-id="f8f3c-199">Cílem této části je vytvoření uživatele volat Britta Simon na portálu pro správu cloudu pro Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-199">The objective of this section is to create a user called Britta Simon in Cloud Management Portal for Microsoft Azure.</span></span> <span data-ttu-id="f8f3c-200">Spojte se s [portál pro správu cloudu pro tým podpory Microsoft Azure](mailto:jczernuszka@newsignature.com) přidání uživatelů v portálu pro správu cloudu pro účet Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-200">Please work with [Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com) to add the users in the Cloud Management Portal for Microsoft Azure account.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f8f3c-201">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8f3c-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f8f3c-202">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k portálu pro správu cloudu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cloud Management Portal for Microsoft Azure.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f8f3c-204">**Pokud chcete přiřadit Britta Simon portál pro správu cloudu pro Microsoft Azure, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f8f3c-204">**To assign Britta Simon to Cloud Management Portal for Microsoft Azure, perform the following steps:**</span></span>

1. <span data-ttu-id="f8f3c-205">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f8f3c-207">V seznamu aplikací vyberte **portál pro správu cloudu pro Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-207">In the applications list, select **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_app.png) 

3. <span data-ttu-id="f8f3c-209">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f8f3c-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-211">Click **Add** button.</span></span> <span data-ttu-id="f8f3c-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f8f3c-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f8f3c-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f8f3c-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f8f3c-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8f3c-217">Testing single sign-on</span></span>

<span data-ttu-id="f8f3c-218">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-218">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="f8f3c-219">Když kliknete na portálu pro správu cloudu pro dlaždice Microsoft Azure na přístupovém panelu, jste měli získat automaticky přihlášení k portálu pro správu cloudu pro aplikaci Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8f3c-219">When you click the Cloud Management Portal for Microsoft Azure tile in the Access Panel, you should get automatically signed-on to your Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="f8f3c-220">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f8f3c-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8f3c-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f8f3c-221">Additional resources</span></span>

* [<span data-ttu-id="f8f3c-222">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f8f3c-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f8f3c-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f8f3c-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png

