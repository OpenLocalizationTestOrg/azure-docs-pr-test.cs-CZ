---
title: 'Kurz: Azure Active Directory integrace s AnswerHub | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a AnswerHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a1c9cc5d7a2ebe28e9fb7e0e6ed8e3d393873ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a><span data-ttu-id="ff8fe-103">Kurz: Azure Active Directory integrace s AnswerHub</span><span class="sxs-lookup"><span data-stu-id="ff8fe-103">Tutorial: Azure Active Directory integration with AnswerHub</span></span>

<span data-ttu-id="ff8fe-104">V tomto kurzu zjistěte, jak integrovat AnswerHub s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ff8fe-104">In this tutorial, you learn how to integrate AnswerHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ff8fe-105">Integrace AnswerHub s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-105">Integrating AnswerHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ff8fe-106">Můžete řídit ve službě Azure AD, který má přístup k AnswerHub</span><span class="sxs-lookup"><span data-stu-id="ff8fe-106">You can control in Azure AD who has access to AnswerHub</span></span>
- <span data-ttu-id="ff8fe-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k AnswerHub (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff8fe-107">You can enable your users to automatically get signed-on to AnswerHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ff8fe-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ff8fe-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ff8fe-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff8fe-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff8fe-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff8fe-110">Prerequisites</span></span>

<span data-ttu-id="ff8fe-111">Konfigurace integrace Azure AD s AnswerHub, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-111">To configure Azure AD integration with AnswerHub, you need the following items:</span></span>

- <span data-ttu-id="ff8fe-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff8fe-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ff8fe-113">AnswerHub jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ff8fe-113">An AnswerHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff8fe-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ff8fe-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ff8fe-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ff8fe-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff8fe-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ff8fe-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ff8fe-118">Scenario description</span></span>
<span data-ttu-id="ff8fe-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ff8fe-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ff8fe-121">Přidání AnswerHub z Galerie</span><span class="sxs-lookup"><span data-stu-id="ff8fe-121">Adding AnswerHub from the gallery</span></span>
2. <span data-ttu-id="ff8fe-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff8fe-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-answerhub-from-the-gallery"></a><span data-ttu-id="ff8fe-123">Přidání AnswerHub z Galerie</span><span class="sxs-lookup"><span data-stu-id="ff8fe-123">Adding AnswerHub from the gallery</span></span>
<span data-ttu-id="ff8fe-124">Při konfiguraci integrace AnswerHub do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS AnswerHub z galerie.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-124">To configure the integration of AnswerHub into Azure AD, you need to add AnswerHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ff8fe-125">**Pokud chcete přidat AnswerHub z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff8fe-125">**To add AnswerHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ff8fe-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ff8fe-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ff8fe-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ff8fe-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ff8fe-133">Do vyhledávacího pole zadejte **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-133">In the search box, type **AnswerHub**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. <span data-ttu-id="ff8fe-135">Na panelu výsledků vyberte **AnswerHub**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-135">In the results panel, select **AnswerHub**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff8fe-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff8fe-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff8fe-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s AnswerHub podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ff8fe-138">In this section, you configure and test Azure AD single sign-on with AnswerHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ff8fe-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v AnswerHub je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AnswerHub is to a user in Azure AD.</span></span> <span data-ttu-id="ff8fe-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v AnswerHub musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-140">In other words, a link relationship between an Azure AD user and the related user in AnswerHub needs to be established.</span></span>

<span data-ttu-id="ff8fe-141">V AnswerHub, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-141">In AnswerHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ff8fe-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s AnswerHub, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-142">To configure and test Azure AD single sign-on with AnswerHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ff8fe-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ff8fe-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff8fe-145">**[Vytváření testovacího uživatele AnswerHub](#creating-an-answerhub-test-user)**  – Pokud chcete mít protějšek Britta Simon v AnswerHub propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-145">**[Creating an AnswerHub test user](#creating-an-answerhub-test-user)** - to have a counterpart of Britta Simon in AnswerHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ff8fe-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff8fe-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff8fe-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff8fe-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ff8fe-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AnswerHub application.</span></span>

<span data-ttu-id="ff8fe-150">**Ke konfiguraci Azure AD jednotné přihlašování s AnswerHub, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff8fe-150">**To configure Azure AD single sign-on with AnswerHub, perform the following steps:**</span></span>

1. <span data-ttu-id="ff8fe-151">Na portálu Azure na **AnswerHub** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-151">In the Azure portal, on the **AnswerHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ff8fe-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. <span data-ttu-id="ff8fe-155">Na **AnswerHub domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-155">On the **AnswerHub Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    <span data-ttu-id="ff8fe-157">a.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-157">a.</span></span> <span data-ttu-id="ff8fe-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="ff8fe-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.answerhub.com`</span></span>

    <span data-ttu-id="ff8fe-159">b.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-159">b.</span></span> <span data-ttu-id="ff8fe-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="ff8fe-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company>.answerhub.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ff8fe-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-161">These values are not real.</span></span> <span data-ttu-id="ff8fe-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ff8fe-163">Obraťte se na [tým podpory AnswerHub klienta](mailto:success@answerhub.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-163">Contact [AnswerHub Client support team](mailto:success@answerhub.com) to get these values.</span></span> 
 
4. <span data-ttu-id="ff8fe-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. <span data-ttu-id="ff8fe-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ff8fe-168">Na **AnswerHub konfigurace** klikněte na tlačítko **konfigurace AnswerHub** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-168">On the **AnswerHub Configuration** section, click **Configure AnswerHub** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ff8fe-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ff8fe-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. <span data-ttu-id="ff8fe-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-171">In a different web browser window, log into your AnswerHub company site as an administrator.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="ff8fe-172">Pokud potřebujete pomoc s konfigurací AnswerHub, obraťte se na [tým podpory pro AnswerHub](mailto:success@answerhub.com.).</span><span class="sxs-lookup"><span data-stu-id="ff8fe-172">If you need help configuring AnswerHub, contact [AnswerHub's support team](mailto:success@answerhub.com.).</span></span>
   
8. <span data-ttu-id="ff8fe-173">Přejděte na **správy**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-173">Go to **Administration**.</span></span>

9. <span data-ttu-id="ff8fe-174">Klikněte **uživatelů a skupin** kartě.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-174">Click the **User and Group** tab.</span></span>

10. <span data-ttu-id="ff8fe-175">V navigačním podokně na levé straně v **sociálních nastavení** klikněte na tlačítko **SAML instalace**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-175">In the navigation pane on the left side, in the **Social Settings** section, click **SAML Setup**.</span></span>

11. <span data-ttu-id="ff8fe-176">Klikněte na tlačítko **IDP konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-176">Click **IDP Config** tab.</span></span>

12. <span data-ttu-id="ff8fe-177">Na **IDP konfigurace** kartu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-177">On the **IDP Config** tab, perform the following steps:</span></span>

     <span data-ttu-id="ff8fe-178">![Instalační program SAML](./media/active-directory-saas-answerhub-tutorial/ic785172.png "instalace SAML")</span><span class="sxs-lookup"><span data-stu-id="ff8fe-178">![SAML Setup](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML Setup")</span></span>  
  
     <span data-ttu-id="ff8fe-179">a.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-179">a.</span></span> <span data-ttu-id="ff8fe-180">V **IDP přihlašovací adresa URL** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-180">In **IDP Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
     <span data-ttu-id="ff8fe-181">b.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-181">b.</span></span> <span data-ttu-id="ff8fe-182">V **adresy URL odhlašovací IDP** textovému poli, vložte **Sign-Out URL** hodnotu, která jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-182">In **IDP Logout URL** textbox, paste **Sign-Out URL** value which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="ff8fe-183">c.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-183">c.</span></span> <span data-ttu-id="ff8fe-184">V **formát identifikátor názvu IDP** textovému poli, zadejte uživatele identifikátor stejná hodnota jako vybrané v portálu Azure v **uživatelské atributy** části.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-184">In **IDP Name Identifier Format** textbox, enter the user Identifier value same as selected in Azure portal in **User Attributes** section.</span></span>
  
     <span data-ttu-id="ff8fe-185">d.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-185">d.</span></span> <span data-ttu-id="ff8fe-186">Klikněte na tlačítko **klíčů a certifikátů**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-186">Click **Keys and Certificates**.</span></span>

13. <span data-ttu-id="ff8fe-187">Na kartě klíčů a certifikátů proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-187">On the Keys and Certificates tab, perform the following steps:</span></span>
    
     <span data-ttu-id="ff8fe-188">![Klíčů a certifikátů](./media/active-directory-saas-answerhub-tutorial/ic785173.png "klíčů a certifikátů")</span><span class="sxs-lookup"><span data-stu-id="ff8fe-188">![Keys and Certificates](./media/active-directory-saas-answerhub-tutorial/ic785173.png "Keys and Certificates")</span></span>  
 
     <span data-ttu-id="ff8fe-189">a.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-189">a.</span></span> <span data-ttu-id="ff8fe-190">Vaše kódování base-64 kódovaného certifikátu, který jste si stáhli z portálu Azure v poznámkovém bloku otevřete kopírovat obsah ho do schránky a vložte jej do **IDP veřejný klíč (x 509 formátu)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-190">Open your base-64 encoded certificate which you have downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **IDP Public Key (x509 Format)** textbox.</span></span>
  
     <span data-ttu-id="ff8fe-191">b.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-191">b.</span></span> <span data-ttu-id="ff8fe-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-192">Click **Save**.</span></span>

14. <span data-ttu-id="ff8fe-193">Na **IDP konfigurace** , klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-193">On the **IDP Config** tab, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="ff8fe-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ff8fe-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ff8fe-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ff8fe-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ff8fe-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff8fe-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff8fe-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff8fe-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ff8fe-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff8fe-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ff8fe-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ff8fe-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ff8fe-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ff8fe-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff8fe-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ff8fe-209">a.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-209">a.</span></span> <span data-ttu-id="ff8fe-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ff8fe-211">b.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-211">b.</span></span> <span data-ttu-id="ff8fe-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ff8fe-213">c.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-213">c.</span></span> <span data-ttu-id="ff8fe-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ff8fe-215">d.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-215">d.</span></span> <span data-ttu-id="ff8fe-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-216">Click **Create**.</span></span>
 
### <a name="creating-an-answerhub-test-user"></a><span data-ttu-id="ff8fe-217">Vytváření testovacího uživatele AnswerHub</span><span class="sxs-lookup"><span data-stu-id="ff8fe-217">Creating an AnswerHub test user</span></span>

<span data-ttu-id="ff8fe-218">Pokud chcete povolit uživatelům Azure AD přihlášení k AnswerHub, musí být zřízená do AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-218">To enable Azure AD users to log in to AnswerHub, they must be provisioned into AnswerHub.</span></span>  
<span data-ttu-id="ff8fe-219">V případě AnswerHub zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-219">In the case of AnswerHub, provisioning is a manual task.</span></span>

<span data-ttu-id="ff8fe-220">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff8fe-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="ff8fe-221">Přihlaste se k vaší **AnswerHub** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-221">Log in to your **AnswerHub** company site as administrator.</span></span>

2. <span data-ttu-id="ff8fe-222">Přejděte na **správy**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-222">Go to **Administration**.</span></span>

3. <span data-ttu-id="ff8fe-223">Klikněte **uživatelé a skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-223">Click the **Users & Groups** tab.</span></span>

4. <span data-ttu-id="ff8fe-224">V navigačním podokně na levé straně v **spravovat uživatele** klikněte na tlačítko **vytvoření nebo import uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-224">In the navigation pane on the left side, in the **Manage Users** section, click **Create or import users**.</span></span>
   
   <span data-ttu-id="ff8fe-225">![Uživatelé a skupiny](./media/active-directory-saas-answerhub-tutorial/ic785175.png "uživatelé a skupiny")</span><span class="sxs-lookup"><span data-stu-id="ff8fe-225">![Users & Groups](./media/active-directory-saas-answerhub-tutorial/ic785175.png "Users & Groups")</span></span>

5. <span data-ttu-id="ff8fe-226">Typ **e-mailová adresa**, **uživatelské jméno** a **heslo** platného účtu Azure Active Directory, kterou chcete zřídit do související textová pole a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-226">Type the **Email address**, **Username** and **Password** of a valid Azure Active Directory account you want to provision into the related textboxes, and then click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="ff8fe-227">Můžete použít všechny ostatní AnswerHub uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované AnswerHub zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-227">You can use any other AnswerHub user account creation tools or APIs provided by AnswerHub to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ff8fe-228">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff8fe-228">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ff8fe-229">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AnswerHub.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ff8fe-231">**Pokud chcete přiřadit Britta Simon AnswerHub, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff8fe-231">**To assign Britta Simon to AnswerHub, perform the following steps:**</span></span>

1. <span data-ttu-id="ff8fe-232">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ff8fe-234">V seznamu aplikací vyberte **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-234">In the applications list, select **AnswerHub**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. <span data-ttu-id="ff8fe-236">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ff8fe-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-238">Click **Add** button.</span></span> <span data-ttu-id="ff8fe-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ff8fe-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ff8fe-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ff8fe-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ff8fe-244">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff8fe-244">Testing single sign-on</span></span>

<span data-ttu-id="ff8fe-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ff8fe-246">Když kliknete na dlaždici AnswerHub na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="ff8fe-246">When you click the AnswerHub tile in the Access Panel, you should get automatically signed-on to your AnswerHub application.</span></span>
<span data-ttu-id="ff8fe-247">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ff8fe-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff8fe-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ff8fe-248">Additional resources</span></span>

* [<span data-ttu-id="ff8fe-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff8fe-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff8fe-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ff8fe-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

