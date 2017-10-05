---
title: 'Kurz: Azure Active Directory integrace s SuccessFactors | Microsoft Docs'
description: "Další informace o použití SuccessFactors s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e85a38ccbe25263ac42bc76351416b023fb77c87
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a><span data-ttu-id="b9edf-103">Kurz: Azure Active Directory integrace s SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="b9edf-103">Tutorial: Azure Active Directory integration with SuccessFactors</span></span>
<span data-ttu-id="b9edf-104">Cílem tohoto kurzu je ukazují, jak integrovat SuccessFactors s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b9edf-104">The objective of this tutorial is to show you how to integrate SuccessFactors with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b9edf-105">Integrace SuccessFactors s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b9edf-105">Integrating SuccessFactors with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="b9edf-106">Můžete řídit ve službě Azure AD, který má přístup k SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="b9edf-106">You can control in Azure AD who has access to SuccessFactors</span></span>
* <span data-ttu-id="b9edf-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k SuccessFactors (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9edf-107">You can enable your users to automatically get signed-on to SuccessFactors (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="b9edf-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="b9edf-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="b9edf-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b9edf-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9edf-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b9edf-110">Prerequisites</span></span>
<span data-ttu-id="b9edf-111">Konfigurace integrace Azure AD s SuccessFactors, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b9edf-111">To configure Azure AD integration with SuccessFactors, you need the following items:</span></span>

* <span data-ttu-id="b9edf-112">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="b9edf-112">A valid Azure subscription</span></span>
* <span data-ttu-id="b9edf-113">Klienta v SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="b9edf-113">A tenant in SuccessFactors</span></span>

> [!NOTE]
> <span data-ttu-id="b9edf-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9edf-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="b9edf-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b9edf-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="b9edf-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="b9edf-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="b9edf-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b9edf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b9edf-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b9edf-118">Scenario description</span></span>
<span data-ttu-id="b9edf-119">Cílem tohoto kurzu je vám umožní testování Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9edf-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="b9edf-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b9edf-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b9edf-121">Přidání SuccessFactors z Galerie</span><span class="sxs-lookup"><span data-stu-id="b9edf-121">Adding SuccessFactors from the gallery</span></span>
2. <span data-ttu-id="b9edf-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9edf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-successfactors-from-the-gallery"></a><span data-ttu-id="b9edf-123">Přidání SuccessFactors z Galerie</span><span class="sxs-lookup"><span data-stu-id="b9edf-123">Adding SuccessFactors from the gallery</span></span>
<span data-ttu-id="b9edf-124">Při konfiguraci integrace SuccessFactors do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS SuccessFactors z galerie.</span><span class="sxs-lookup"><span data-stu-id="b9edf-124">To configure the integration of SuccessFactors into Azure AD, you need to add SuccessFactors from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b9edf-125">**Pokud chcete přidat SuccessFactors z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b9edf-125">**To add SuccessFactors from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b9edf-126">V portálu Azure classic, na levém navigačním panelu klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-126">In the Azure classic portal, on the left navigation panel, click **Active Directory**.</span></span>
   
    ![Konfigurace jednotného přihlašování][1]
2. <span data-ttu-id="b9edf-128">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="b9edf-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="b9edf-129">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="b9edf-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Konfigurace jednotného přihlašování][2]
4. <span data-ttu-id="b9edf-131">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="b9edf-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Aplikace][3]
5. <span data-ttu-id="b9edf-133">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Konfigurace jednotného přihlašování][4]
6. <span data-ttu-id="b9edf-135">V **vyhledávacího pole**, typ **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-135">In the **search box**, type **SuccessFactors**.</span></span>
   
    ![Konfigurace jednotného přihlašování][5]
7. <span data-ttu-id="b9edf-137">Na panelu výsledků vyberte **SuccessFactors**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="b9edf-137">In the results panel, select **SuccessFactors**, and then click **Complete** to add the application.</span></span>
   
    ![Konfigurace jednotného přihlašování][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b9edf-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9edf-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b9edf-140">Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování s SuccessFactors podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b9edf-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with SuccessFactors based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b9edf-141">Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v SuccessFactors pro uživatele ve službě Azure AD je Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b9edf-141">For single sign-on to work, Azure AD needs to know what the counterpart user in SuccessFactors to an user in Azure AD is.</span></span> <span data-ttu-id="b9edf-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SuccessFactors musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b9edf-142">In other words, a link relationship between an Azure AD user and the related user in SuccessFactors needs to be established.</span></span>

<span data-ttu-id="b9edf-143">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="b9edf-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SuccessFactors.</span></span>

<span data-ttu-id="b9edf-144">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SuccessFactors, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b9edf-144">To configure and test Azure AD single sign-on with SuccessFactors, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b9edf-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b9edf-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b9edf-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9edf-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b9edf-147">**[Vytvoření zkušebního uživatele SuccessFactors](#creating-a-successfactors-test-user)**  – Pokud chcete mít protějšek Britta Simon v SuccessFactors propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="b9edf-147">**[Creating a SuccessFactors test user](#creating-a-successfactors-test-user)** - to have a counterpart of Britta Simon in SuccessFactors that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="b9edf-148">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b9edf-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b9edf-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b9edf-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b9edf-150">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9edf-150">Configuring Azure AD single sign-on</span></span>
<span data-ttu-id="b9edf-151">V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="b9edf-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your SuccessFactors application.</span></span>

<span data-ttu-id="b9edf-152">**Ke konfiguraci Azure AD jednotné přihlašování s SuccessFactors, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b9edf-152">**To configure Azure AD single sign-on with SuccessFactors, perform the following steps:**</span></span>

1. <span data-ttu-id="b9edf-153">Na portálu Azure classic na **SuccessFactors** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b9edf-153">In the Azure classic portal, on the **SuccessFactors** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    ![Konfigurace jednotného přihlašování][7]
2. <span data-ttu-id="b9edf-155">Na **jak chcete uživatelům se přihlásit SuccessFactors** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-155">On the **How would you like users to sign on to SuccessFactors** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurace jednotného přihlašování][8]
3. <span data-ttu-id="b9edf-157">Na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-157">On the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
    ![Konfigurace jednotného přihlašování][9]
   
    <span data-ttu-id="b9edf-159">a.</span><span class="sxs-lookup"><span data-stu-id="b9edf-159">a.</span></span> <span data-ttu-id="b9edf-160">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí jedné z následujících kritérií:</span><span class="sxs-lookup"><span data-stu-id="b9edf-160">In the **Sign On URL** textbox, type a URL using one of the following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    <span data-ttu-id="b9edf-161">b.</span><span class="sxs-lookup"><span data-stu-id="b9edf-161">b.</span></span> <span data-ttu-id="b9edf-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí jedné z následujících kritérií:</span><span class="sxs-lookup"><span data-stu-id="b9edf-162">In the **Reply URL** textbox, type a URL using one of the following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    <span data-ttu-id="b9edf-163">c.</span><span class="sxs-lookup"><span data-stu-id="b9edf-163">c.</span></span> <span data-ttu-id="b9edf-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-164">Click **Next**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="b9edf-165">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b9edf-165">Please note that these are not the real values.</span></span> <span data-ttu-id="b9edf-166">Budete muset aktualizovat tyto hodnoty se skutečné přihlašovací adresa URL a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b9edf-166">You have to update these values with the actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="b9edf-167">Chcete-li získat tyto hodnoty, obraťte se na [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="b9edf-167">To get these values, contact [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

1. <span data-ttu-id="b9edf-168">Na **nakonfigurovat jednotné přihlašování v SuccessFactors** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b9edf-168">On the **Configure single sign-on at SuccessFactors** page, click **Download certificate**, and then save the certificate file locally on your computer.</span></span>
   
    ![Konfigurace jednotného přihlašování][10]

2. <span data-ttu-id="b9edf-170">V okně prohlížeče jiný web, přihlaste se k vaší **portál pro správu SuccessFactors** jako správce.</span><span class="sxs-lookup"><span data-stu-id="b9edf-170">In a different web browser window, log into your **SuccessFactors admin portal** as an administrator.</span></span>

3. <span data-ttu-id="b9edf-171">Navštivte **zabezpečení aplikací** a nativní pro **jeden znak na funkci**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-171">Visit **Application Security** and native to **Single Sign On Feature**.</span></span> 

4. <span data-ttu-id="b9edf-172">Umístění všechny hodnoty v **resetovat tokenu** a klikněte na tlačítko **uložit tokenu** umožňující jednotného přihlašování SAML.</span><span class="sxs-lookup"><span data-stu-id="b9edf-172">Place any value in the **Reset Token** and click **Save Token** to enable SAML SSO.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace][11]

    > [!NOTE] 
    > <span data-ttu-id="b9edf-174">Tato hodnota se právě používá jako přepínač zapnout nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="b9edf-174">This value is just used as the on/off switch.</span></span> <span data-ttu-id="b9edf-175">Pokud je uloženo žádnou hodnotu, jednotné přihlašování SAML je ON.</span><span class="sxs-lookup"><span data-stu-id="b9edf-175">If any value is saved, the SAML SSO is ON.</span></span> <span data-ttu-id="b9edf-176">Pokud je uloženo na prázdnou hodnotu jednotné přihlašování SAML je VYPNUTÝ.</span><span class="sxs-lookup"><span data-stu-id="b9edf-176">If a blank value is saved the SAML SSO is OFF.</span></span>

1. <span data-ttu-id="b9edf-177">Nativní na následující snímek obrazovky a proveďte následující akce.</span><span class="sxs-lookup"><span data-stu-id="b9edf-177">Native to below screenshot and perform the following actions.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace][12]
   
    <span data-ttu-id="b9edf-179">a.</span><span class="sxs-lookup"><span data-stu-id="b9edf-179">a.</span></span> <span data-ttu-id="b9edf-180">Vyberte **jednotné přihlašování SAML v2** přepínač</span><span class="sxs-lookup"><span data-stu-id="b9edf-180">Select the **SAML v2 SSO** Radio Button</span></span>
   
    <span data-ttu-id="b9edf-181">b.</span><span class="sxs-lookup"><span data-stu-id="b9edf-181">b.</span></span> <span data-ttu-id="b9edf-182">Nastavte Name(e.g. SAml issuer + company name) strany potvrzující SAML.</span><span class="sxs-lookup"><span data-stu-id="b9edf-182">Set the SAML Asserting Party Name(e.g. SAml issuer + company name).</span></span>
   
    <span data-ttu-id="b9edf-183">c.</span><span class="sxs-lookup"><span data-stu-id="b9edf-183">c.</span></span> <span data-ttu-id="b9edf-184">V **SAML vystavitele** textbox vložte hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b9edf-184">In the **SAML Issuer** textbox put the value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="b9edf-185">d.</span><span class="sxs-lookup"><span data-stu-id="b9edf-185">d.</span></span> <span data-ttu-id="b9edf-186">Vyberte **odpovědi (zákazníka generované nebo IdP/přístupový bod)** jako **vyžadují povinné podpis**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-186">Select **Response(Customer Generated/IdP/AP)** as **Require Mandatory Signature**.</span></span>
   
    <span data-ttu-id="b9edf-187">e.</span><span class="sxs-lookup"><span data-stu-id="b9edf-187">e.</span></span> <span data-ttu-id="b9edf-188">Vyberte **povoleno** jako **povolit SAML příznak**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-188">Select **Enabled** as **Enable SAML Flag**.</span></span>
   
    <span data-ttu-id="b9edf-189">f.</span><span class="sxs-lookup"><span data-stu-id="b9edf-189">f.</span></span> <span data-ttu-id="b9edf-190">Vyberte **ne** jako **podpisu požadavku přihlášení (SF generované/SP/RP)**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-190">Select **No** as **Login Request Signature(SF Generated/SP/RP)**.</span></span>
   
    <span data-ttu-id="b9edf-191">g.</span><span class="sxs-lookup"><span data-stu-id="b9edf-191">g.</span></span> <span data-ttu-id="b9edf-192">Vyberte **profil prohlížeče nebo Pozálohovacího** jako **SAML profil**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-192">Select **Browser/Post Profile** as **SAML Profile**.</span></span>
   
    <span data-ttu-id="b9edf-193">h.</span><span class="sxs-lookup"><span data-stu-id="b9edf-193">h.</span></span> <span data-ttu-id="b9edf-194">Vyberte **ne** jako **vynutit období platný certifikát**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-194">Select **No** as **Enforce Certificate Valid Period**.</span></span>
   
    <span data-ttu-id="b9edf-195">i.</span><span class="sxs-lookup"><span data-stu-id="b9edf-195">i.</span></span> <span data-ttu-id="b9edf-196">Kopírovat obsah souboru stažený certifikát a vložte ji do **ověření certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b9edf-196">Copy the content of the downloaded certificate file, and then paste it into the **SAML Verifying Certificate** textbox.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b9edf-197">Obsahu certifikátu musí začínat certifikátu a koncovou značkou certifikátu.</span><span class="sxs-lookup"><span data-stu-id="b9edf-197">The certificate content must have begin certificate and end certificate tags.</span></span>

1. <span data-ttu-id="b9edf-198">Přejděte do SAML V2 a pak proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b9edf-198">Navigate to SAML V2, and then perform the following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace][13]
   
    <span data-ttu-id="b9edf-200">a.</span><span class="sxs-lookup"><span data-stu-id="b9edf-200">a.</span></span> <span data-ttu-id="b9edf-201">Vyberte **Ano** jako **podporu spouštěná SP globální odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-201">Select **Yes** as **Support SP-initiated Global Logout**.</span></span>
   
    <span data-ttu-id="b9edf-202">b.</span><span class="sxs-lookup"><span data-stu-id="b9edf-202">b.</span></span> <span data-ttu-id="b9edf-203">V **globální odhlášení adresa URL služby (LogoutRequest cíl)** textbox vložte hodnotu **vzdálené adresy URL odhlašovací** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b9edf-203">In the **Global Logout Service URL (LogoutRequest destination)** textbox put the value of **Remote Logout URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="b9edf-204">c.</span><span class="sxs-lookup"><span data-stu-id="b9edf-204">c.</span></span> <span data-ttu-id="b9edf-205">Vyberte **ne** jako **vyžadují sp musí zašifrovat všechny element NameID**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-205">Select **No** as **Require sp must encrypt all NameID element**.</span></span>
   
    <span data-ttu-id="b9edf-206">d.</span><span class="sxs-lookup"><span data-stu-id="b9edf-206">d.</span></span> <span data-ttu-id="b9edf-207">Vyberte **neurčené** jako **NameID formátu**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-207">Select **unspecified** as **NameID Format**.</span></span>
   
    <span data-ttu-id="b9edf-208">e.</span><span class="sxs-lookup"><span data-stu-id="b9edf-208">e.</span></span> <span data-ttu-id="b9edf-209">Vyberte **Ano** jako **povolit sp iniciované přihlášení (AuthnRequest)**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-209">Select **Yes** as **Enable sp initiated login (AuthnRequest)**.</span></span>
   
    <span data-ttu-id="b9edf-210">f.</span><span class="sxs-lookup"><span data-stu-id="b9edf-210">f.</span></span> <span data-ttu-id="b9edf-211">V **odeslán požadavek na jako vydavatel společnosti** textbox vložte hodnotu **vzdálené adresy URL pro přihlášení** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b9edf-211">In the **Send request as Company-Wide issuer** textbox put the value of **Remote Login URL** from Azure AD application configuration wizard.</span></span>
2. <span data-ttu-id="b9edf-212">Proveďte tyto kroky, pokud chcete, aby uživatelská jména přihlášení malá a velká písmena,.</span><span class="sxs-lookup"><span data-stu-id="b9edf-212">Perform these steps if you want to make the login usernames Case Insensitive, .</span></span>
   
    <span data-ttu-id="b9edf-213">a.</span><span class="sxs-lookup"><span data-stu-id="b9edf-213">a.</span></span> <span data-ttu-id="b9edf-214">Navštivte **nastavení společnosti**(u dolního).</span><span class="sxs-lookup"><span data-stu-id="b9edf-214">Visit **Company Settings**(near the bottom).</span></span>
   
    <span data-ttu-id="b9edf-215">b.</span><span class="sxs-lookup"><span data-stu-id="b9edf-215">b.</span></span> <span data-ttu-id="b9edf-216">Zaškrtněte políčko téměř **povolit uživatelské jméno bez rozlišování**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-216">select checkbox near **Enable Non-Case-Sensitive Username**.</span></span>
   
    <span data-ttu-id="b9edf-217">c.Click **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-217">c.Click **Save**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][29]

    > [!NOTE] 
    > <span data-ttu-id="b9edf-219">Pokud se pokusíte povolit tuto funkci, systém zkontroluje, pokud vytvoří duplicitní SAML přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="b9edf-219">If you try to enable this, the system checks if it will create a duplicate SAML login name.</span></span> <span data-ttu-id="b9edf-220">Například pokud zákazník má uživatelských jmen uživatel1 a user1.</span><span class="sxs-lookup"><span data-stu-id="b9edf-220">For example if the customer has usernames User1 and user1.</span></span> <span data-ttu-id="b9edf-221">Pořízení rychle rozlišování umožňuje tyto duplikáty.</span><span class="sxs-lookup"><span data-stu-id="b9edf-221">Taking away case sensitivity makes these duplicates.</span></span> <span data-ttu-id="b9edf-222">Systém vám poskytne chybovou zprávu a neumožní funkci.</span><span class="sxs-lookup"><span data-stu-id="b9edf-222">The system will give you an error message and will not enable the feature.</span></span> <span data-ttu-id="b9edf-223">Zákazník muset změnit jeden z uživatelských jménech, je napsaný ve skutečnosti jiný.</span><span class="sxs-lookup"><span data-stu-id="b9edf-223">The customer will need to change one of the usernames so it’s actually spelled different.</span></span> 

1. <span data-ttu-id="b9edf-224">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b9edf-224">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    ![Aplikace][14]
2. <span data-ttu-id="b9edf-226">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-226">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    ![Aplikace][15]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b9edf-228">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9edf-228">Creating an Azure AD test user</span></span>
<span data-ttu-id="b9edf-229">Cílem této části je vytvoření zkušebního uživatele na portálu classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9edf-229">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][16]

<span data-ttu-id="b9edf-231">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b9edf-231">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b9edf-232">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-232">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][17]
2. <span data-ttu-id="b9edf-234">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="b9edf-234">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="b9edf-235">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-235">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][18]
4. <span data-ttu-id="b9edf-237">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-237">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][19]
5. <span data-ttu-id="b9edf-239">Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b9edf-239">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][20]
   
    <span data-ttu-id="b9edf-241">a.</span><span class="sxs-lookup"><span data-stu-id="b9edf-241">a.</span></span> <span data-ttu-id="b9edf-242">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="b9edf-242">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="b9edf-243">b.</span><span class="sxs-lookup"><span data-stu-id="b9edf-243">b.</span></span> <span data-ttu-id="b9edf-244">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-244">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="b9edf-245">c.</span><span class="sxs-lookup"><span data-stu-id="b9edf-245">c.</span></span> <span data-ttu-id="b9edf-246">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-246">Click **Next**.</span></span>
6. <span data-ttu-id="b9edf-247">Na **profil uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b9edf-247">On the **User Profile** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][21]
   
    <span data-ttu-id="b9edf-249">a.</span><span class="sxs-lookup"><span data-stu-id="b9edf-249">a.</span></span> <span data-ttu-id="b9edf-250">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-250">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="b9edf-251">b.</span><span class="sxs-lookup"><span data-stu-id="b9edf-251">b.</span></span> <span data-ttu-id="b9edf-252">V **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-252">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="b9edf-253">c.</span><span class="sxs-lookup"><span data-stu-id="b9edf-253">c.</span></span> <span data-ttu-id="b9edf-254">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-254">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="b9edf-255">d.</span><span class="sxs-lookup"><span data-stu-id="b9edf-255">d.</span></span> <span data-ttu-id="b9edf-256">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-256">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="b9edf-257">e.</span><span class="sxs-lookup"><span data-stu-id="b9edf-257">e.</span></span> <span data-ttu-id="b9edf-258">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-258">Click **Next**.</span></span>
7. <span data-ttu-id="b9edf-259">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-259">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][22]
8. <span data-ttu-id="b9edf-261">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b9edf-261">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][23]
   
    <span data-ttu-id="b9edf-263">a.</span><span class="sxs-lookup"><span data-stu-id="b9edf-263">a.</span></span> <span data-ttu-id="b9edf-264">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-264">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="b9edf-265">b.</span><span class="sxs-lookup"><span data-stu-id="b9edf-265">b.</span></span> <span data-ttu-id="b9edf-266">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-266">Click **Complete**.</span></span>  

### <a name="creating-a-successfactors-test-user"></a><span data-ttu-id="b9edf-267">Vytvoření zkušebního uživatele SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="b9edf-267">Creating a SuccessFactors test user</span></span>
<span data-ttu-id="b9edf-268">Pokud chcete povolit uživatelům Azure AD přihlášení do SuccessFactors, musí být zřízená do SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="b9edf-268">In order to enable Azure AD users to log into SuccessFactors, they must be provisioned into SuccessFactors.</span></span>  
<span data-ttu-id="b9edf-269">V případě SuccessFactors zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b9edf-269">In the case of SuccessFactors, provisioning is a manual task.</span></span>

<span data-ttu-id="b9edf-270">Chcete-li získat uživatelé vytvoření ve SuccessFactors, budete muset kontaktovat [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="b9edf-270">To get users created in SuccessFactors, you need to contact the [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b9edf-271">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9edf-271">Assigning the Azure AD test user</span></span>
<span data-ttu-id="b9edf-272">Cílem této části je povolení Britta Simon používat tak, že udělíte přístup k SuccessFactors Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b9edf-272">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to SuccessFactors.</span></span>

![Přiřadit uživatele][24]

<span data-ttu-id="b9edf-274">**Pokud chcete přiřadit Britta Simon SuccessFactors, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b9edf-274">**To assign Britta Simon to SuccessFactors, perform the following steps:**</span></span>

1. <span data-ttu-id="b9edf-275">Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="b9edf-275">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Přiřadit uživatele][25]
2. <span data-ttu-id="b9edf-277">V seznamu aplikací vyberte **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-277">In the applications list, select **SuccessFactors**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][26]
3. <span data-ttu-id="b9edf-279">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-279">In the menu on the top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][27]
4. <span data-ttu-id="b9edf-281">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-281">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="b9edf-282">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="b9edf-282">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][28]

### <a name="testing-single-sign-on"></a><span data-ttu-id="b9edf-284">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b9edf-284">Testing single sign-on</span></span>
<span data-ttu-id="b9edf-285">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b9edf-285">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b9edf-286">Když kliknete na dlaždici SuccessFactors na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="b9edf-286">When you click the SuccessFactors tile in the Access Panel, you should get automatically signed-on to your SuccessFactors application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9edf-287">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b9edf-287">Additional resources</span></span>
* [<span data-ttu-id="b9edf-288">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b9edf-288">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b9edf-289">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b9edf-289">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
