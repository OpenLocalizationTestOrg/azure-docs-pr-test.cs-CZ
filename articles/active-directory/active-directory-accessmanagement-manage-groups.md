---
title: "Správa skupin ve službě Azure Active Directory | Dokumentace Microsoftu"
description: "Postupy při vytváření a správě skupin pro správu uživatelů Azure pomocí služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 2cc2b63312b331a19c61cd7b59a4cac78edf32e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="managing-groups-in-azure-active-directory"></a>Správa skupin ve službě Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [Portál Azure Classic](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Jedna z funkcí správy uživatelů služby Azure Active Directory (Azure AD) je možnost vytváření skupin uživatelů. Pomocí skupiny můžete provádět úlohy správy, jako je přiřazení licencí nebo oprávnění víc uživatelům současně. Skupiny můžete použít také k přiřazení přístupových oprávnění pro:

* prostředky, například objekty v adresáři
* Prostředky, které jsou pro adresář externí, třeba aplikace SaaS, služby Azure, weby SharePoint nebo místní prostředky

Vlastník prostředku může navíc přístup k prostředku přiřadit i skupině služby Azure AD, která patří někomu jinému. Tím získají členové této skupiny přístup k prostředku. Vlastník skupiny potom spravuje členství ve skupině. Vlastník prostředku deleguje vlastníkovi skupiny oprávnění přiřazovat uživatele k jeho prostředku.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. Informace o správě skupin v centru pro správu Azure AD najdete v tématu [Vytvoření skupiny a přidání členů v Azure Active Directory](active-directory-groups-create-azure-portal.md).

## <a name="how-do-i-create-a-group"></a>Vytvoření skupiny
V závislosti na tom, jaké služby má vaše organizace předplacené, můžete vytvořit skupinu pomocí některé z těchto služeb:

* Portál Azure Classic
* Portál účtu služeb Office 365
* Portál účtu Windows Intune

Budeme popisovat úlohy prováděné na portálu Azure Classic. Další informace o používání portálů mimo Azure ke správě adresáře služby Azure AD najdete v článku [Správa adresáře služby Azure AD](active-directory-administer.md).

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.
2. Vyberte kartu **Skupiny**.
3. Vyberte **Přidat skupinu**.
4. V okně **Přidat skupinu** zadejte název a popis skupiny.

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Přidání nebo odebrání jednotlivých uživatelů ve skupině zabezpečení
**Přidání jednotlivého uživatele do skupiny**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.
2. Vyberte kartu **Skupiny**.
3. Otevřete skupinu, do které chcete přidat členy. Pro vybranou skupinu otevřete kartu **Členové**, pokud ještě není zobrazená.
4. Vyberte **Přidat členy**.
5. Na stránce **Přidat členy** vyberte jméno uživatele nebo název skupiny, které chcete přidat jako člena této skupiny. Nezapomeňte toto jméno nebo název přidat i do podokna **Vybraní**.

**Odebrání jednotlivého uživatele ze skupiny**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.
2. Vyberte kartu **Skupiny**.
3. Otevřete skupinu, ze které chcete odebrat členy.
4. Vyberte kartu **Členové**, vyberte jméno člena, kterého chcete z této skupiny odebrat, a potom klikněte na **Odebrat**.
5. V zobrazené výzvě potvrďte, že chcete tohoto člena ze skupiny odebrat.

## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Dynamická správa členství ve skupině
Ve službě Azure AD můžete velmi snadno nastavit jednoduché pravidlo a určit tak uživatele, kteří se mají stát členy skupiny. Jednoduché pravidlo je takové pravidlo, které provádí jenom jedno porovnání. Pokud je třeba některá skupina přiřazená aplikaci SaaS, můžete vytvořit pravidlo, které zajistí přidání uživatelů s názvem pracovní pozice „Obchodní zástupce“. Toto pravidlo potom udělí přístup do této aplikace SaaS všem uživatelům s touto pracovní pozicí ve vašem adresáři.

Když se změní libovolné atributy uživatele, systém vyhodnotí všechna dynamická pravidla skupin v adresáři a zjistí, zda změna uživatele aktivuje nějaké přidání do skupiny nebo odebrání ze skupiny. Pokud uživatel splňuje pravidlo pro skupinu, je do této skupiny přidán jako člen. Pokud již uživatel pravidlo pro skupinu, ve které je členem, nesplňuje, je jeho členství ve skupině odebráno.

> [!NOTE]
> Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365. Vnořené členství ve skupinách se v současné době nepodporuje v případě přiřazování k aplikacím podle skupiny.
>
> Dynamické členství ve skupinách vyžaduje přiřazení licence služby Azure AD Premium následujícím uživatelům:
>
> * správce, který spravuje pravidlo pro skupinu
> * Všichni členové skupiny
>
>

**Povolení dynamického členství ve skupině**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.
2. Vyberte kartu **Skupiny** a otevřete skupinu, kterou chcete upravit.
3. Vyberte kartu **Konfigurace** a potom nastavte možnost **Povolit dynamické členství** na **Ano**.
4. Nastavte jedno jednoduché pravidlo pro skupinu, které bude řídit dynamické členství pro funkce v této skupině. Zkontrolujte, jestli jste vybrali možnost **Přidat uživatele, kde**, a potom vyberte vlastnost uživatele ze seznamu (například oddělení, pracovní funkce atd.),
5. V dalším kroku vyberte podmínku (nerovná se, rovná se, nezačíná, začíná, neobsahuje, obsahuje, neodpovídá, odpovídá).
6. Zadejte hodnotu vybrané vlastnosti uživatele.

Další informace o vytváření *rozšířených* pravidel (která můžou obsahovat několik porovnání) pro dynamické členství ve skupině najdete v článku o [používání atributů k vytvoření rozšířených pravidel](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Další informace
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
