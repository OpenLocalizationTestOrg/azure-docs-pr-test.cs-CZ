---
title: "Azure Active Directory B2C: Resetování hesla pomocí samoobslužné služby | Microsoft Docs"
description: "Téma, který ukazuje, jak nastavit samoobslužné resetování hesla pro uživatele v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 0508868e3b00c5771cc26038a3dd71fde6625a84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a><span data-ttu-id="d947f-103">Azure Active Directory B2C: Nastavení samoobslužného resetování hesla pro uživatele</span><span class="sxs-lookup"><span data-stu-id="d947f-103">Azure Active Directory B2C: Set up self-service password reset for your consumers</span></span>
<span data-ttu-id="d947f-104">Funkce resetování hesla pomocí samoobslužné služby s uživatele (kteří zaregistrovali pro místní účty) mohou resetovat svá hesla na své vlastní.</span><span class="sxs-lookup"><span data-stu-id="d947f-104">With the self-service password reset feature, your consumers (who have signed up for local accounts) can reset their passwords on their own.</span></span> <span data-ttu-id="d947f-105">To významně snižuje zátěž vašim zaměstnancům technické podpory, zvlášť pokud vaše aplikace nemá milionům příjemcům na základě v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="d947f-105">This significantly reduces the burden on your support staff, especially if your application has millions of consumers using it on a regular basis.</span></span> <span data-ttu-id="d947f-106">V současné době podporujeme jenom pomocí ověřenou e-mailovou adresu jako metodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="d947f-106">Currently, we only support using a verified email address as a recovery method.</span></span> <span data-ttu-id="d947f-107">Přidáme obnovení dodatečné metody (ověřené telefonní číslo, bezpečnostní otázky atd.) v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="d947f-107">We will add additional recovery methods (verified phone number, security questions, etc.) in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="d947f-108">Tento článek se týká hesla pomocí samoobslužné služby resetování použít v kontextu zásady přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d947f-108">This article applies to self-service password reset used in the context of a sign-in policy.</span></span> <span data-ttu-id="d947f-109">Pokud potřebujete zásady resetování plně přizpůsobitelná hesel volána z vaší aplikace, najdete v části [v tomto článku](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="d947f-109">If you need fully customizable password reset policies invoked from your app, see [this article](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span></span>
> 
> 

<span data-ttu-id="d947f-110">Ve výchozím nastavení, nebudou mít adresáře hesla pomocí samoobslužné služby resetování zapnutý.</span><span class="sxs-lookup"><span data-stu-id="d947f-110">By default, your directory will not have self-service password reset turned on.</span></span> <span data-ttu-id="d947f-111">Chcete-li pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d947f-111">Use the following steps to turn it on:</span></span>

1. <span data-ttu-id="d947f-112">Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) jako Správce předplatného.</span><span class="sxs-lookup"><span data-stu-id="d947f-112">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) as the Subscription Administrator.</span></span> <span data-ttu-id="d947f-113">Toto je stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili k vytvoření adresáře.</span><span class="sxs-lookup"><span data-stu-id="d947f-113">This is the same work or school account or the same Microsoft account that you used to create your directory.</span></span>
2. <span data-ttu-id="d947f-114">Přejděte na rozšíření Active Directory v navigačním panelu na levé straně.</span><span class="sxs-lookup"><span data-stu-id="d947f-114">Navigate to the Active Directory extension on the navigation bar on the left side.</span></span>
3. <span data-ttu-id="d947f-115">Najít váš adresář **Directory** kartě a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="d947f-115">Find your directory under the **Directory** tab and click it.</span></span>
4. <span data-ttu-id="d947f-116">Klikněte na kartu **KONFIGUROVAT**.</span><span class="sxs-lookup"><span data-stu-id="d947f-116">Click the **Configure** tab.</span></span>
5. <span data-ttu-id="d947f-117">Přejděte dolů k položce **zásady resetování hesel uživatelů** části a přepnutí **uživatele povolen pro resetování hesla** možnost k **Ano**.</span><span class="sxs-lookup"><span data-stu-id="d947f-117">Scroll down to the **User password reset policy** section and toggle the **Users enabled for password reset** option to **YES**.</span></span> <span data-ttu-id="d947f-118">Všimněte si, že **alternativní e-mailovou adresu** zaškrtnutá možnost; necháte, protože se jedná.</span><span class="sxs-lookup"><span data-stu-id="d947f-118">Notice that the **Alternate Email Address** option is checked; leave it as it is.</span></span>
   
    ![Samoobslužné resetování hesla](./media/active-directory-b2c-reference-sspr/sspr.png)
6. <span data-ttu-id="d947f-120">V dolní části stránky klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d947f-120">Click **Save** at the bottom of the page.</span></span> <span data-ttu-id="d947f-121">Hotovo!</span><span class="sxs-lookup"><span data-stu-id="d947f-121">You're done!</span></span>

<span data-ttu-id="d947f-122">K testování, pomocí funkce "Spustit nyní" na všechny zásady přihlášení, který má místní účty jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="d947f-122">To test, use the "Run now" feature on any sign-in policy that has local accounts as an identity provider.</span></span> <span data-ttu-id="d947f-123">Na přihlášení místní účet stránka (kde zadáte e-mailovou adresu a heslo, nebo uživatelské jméno a heslo), klikněte na tlačítko **nelze získat přístup k účtu?** ověření prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="d947f-123">On the local account sign-in page (where you enter an email address and password, or a username and password), click **Can't access your account?** to verify the consumer experience.</span></span>

> [!NOTE]
> <span data-ttu-id="d947f-124">Na stránkách resetování hesla pomocí samoobslužné služby lze přizpůsobit pomocí [firemního brandingu funkce](../active-directory/active-directory-add-company-branding.md).</span><span class="sxs-lookup"><span data-stu-id="d947f-124">The self-service password reset pages can be customized by using the [company branding feature](../active-directory/active-directory-add-company-branding.md).</span></span>
> 
> 

