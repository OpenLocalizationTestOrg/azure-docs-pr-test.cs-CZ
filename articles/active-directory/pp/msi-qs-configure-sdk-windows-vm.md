---
title: "Postup konfigurace MSI se přiřazené pro virtuální počítač Azure pomocí sady Azure SDK"
description: "Krok podle podrobné pokyny pro konfiguraci virtuálního počítače Azure, pomocí sady Azure SDK přiřazený uživatelem spravované služby Identity (MSI)."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 19b6179803b8de4102818c1522b00e6b4881d0f0
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-sdk"></a>Konfigurace přiřazený uživatelem spravované služby Identity (MSI) pro virtuální počítač, pomocí sady Azure SDK

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identita spravované služby poskytuje Azure služby spravovanou identitu ve službě Azure Active Directory. Tuto identitu můžete použít k ověření služby, které podporují ověřování Azure AD, bez nutnosti přihlašovací údaje ve vašem kódu. 

V tomto článku zjistěte, jak povolit a odebrat MSI se přiřazené pro virtuální počítač Azure, pomocí sady SDK pro Azure.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="azure-sdks-with-msi-support"></a>Azure SDK s podporou MSI 

Azure podporuje více platforem programovací prostřednictvím řady [sady Azure SDK](https://azure.microsoft.com/downloads). Několik z nich bylo aktualizováno pro podporu MSI a zadejte odpovídající ukázky k předvedení využití. Tento seznam je aktualizovat, protože je přidat další podporu:

| Sada SDK | Ukázka |
| --- | ------ | 
| Python | [Vytvoření virtuálního počítače s MSI povoleno](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Vytvoření virtuálního počítače Azure pomocí souboru MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Další postup

- Najdete v části související články v části "Konfigurace MSI pro virtuálního počítače Azure", se dozvíte, jak můžete nakonfigurovat MSI se přiřazený uživatelem na virtuální počítač Azure.

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.
