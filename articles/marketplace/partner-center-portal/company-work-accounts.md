---
title: Pracovní účty společnosti a partnerské Centrum
description: Jak ověřit, jestli má vaše společnost nastavený pracovní účet s Microsoftem, vytvořte nový pracovní účet nebo nastavte víc pracovních účtů pro použití s partnerským centrem.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: parthpandyaMSFT
ms.author: parthp
ms.date: 05/30/2019
ms.openlocfilehash: 6fca3429ab96a63b01eaa7e9d1f0ae83dc46e9c7
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119748"
---
# <a name="company-work-accounts-and-partner-center"></a>Pracovní účty společnosti a partnerské Centrum

Partnerské centrum používá pracovní účty společnosti, označované také jako klienti Azure Active Directory (AD), ke správě přístupu k účtu pro více uživatelů, řízení oprávnění, skupin hostitelů a aplikací a údržbě dat profilu. Když propojíte doménu firemního e-mailového účtu s vaším účtem partnerského centra, zaměstnanci vaší společnosti se můžou přihlásit do partnerského centra a spravovat nabídky Marketplace pomocí vlastních uživatelských jmen a hesel pracovního účtu.

## <a name="check-whether-your-company-already-has-a-work-account"></a>Ověřte, jestli vaše společnost už má pracovní účet.

Pokud se vaše společnost přihlásila k odběru cloudové služby Microsoftu, jako je Azure, Microsoft Intune nebo Office 365, pak už máte pracovní e-mailový účet (označovaný také jako Azure Active Directory tenant), který se dá použít s partnerským centrem.

Pomocí těchto kroků zkontrolujete:
1. Přihlaste se na portál pro správu Azure na adrese https://portal.azure.com .
2. V navigační nabídce vlevo vyberte **Azure Active Directory** a pak vyberte **vlastní názvy domén**.
3. Pokud již máte pracovní účet, bude uveden název vaší domény.

Pokud vaše společnost ještě nemá pracovní účet, vytvoří se během procesu registrace v partnerském centru jeden za vás.

## <a name="set-up-multiple-work-accounts"></a>Nastavení více pracovních účtů

Než se rozhodnete použít stávající pracovní účet, vezměte v úvahu, kolik uživatelů v pracovním účtu bude potřebovat přístup k partnerskému centru. Pokud máte uživatele v pracovním účtu, který nebude potřebovat přístup k partnerskému centru, možná budete chtít zvážit vytvoření více pracovních účtů, aby se na určitém účtu reprezentují jenom uživatelé, kteří budou potřebovat přístup k partnerskému centru.

## <a name="create-a-new-work-account"></a>Vytvořit nový pracovní účet

Chcete-li vytvořit nový pracovní účet pro vaši společnost, postupujte podle následujících kroků. Možná budete muset požádat o pomoc od osoby, která má oprávnění správce na účtu Microsoft Azure vaší společnosti.

1. Přihlaste se na web [Microsoft Azure Portal](https://portal.azure.com).
2. V navigační nabídce vlevo vyberte **Azure Active Directory**  ->  **Uživatelé**.
3. Vyberte **Nový uživatel** a vytvořte nový pracovní účet Azure zadáním jména a e-mailové adresy. Ujistěte se, že je **role adresáře** nastavená na hodnotu **uživatel** a v dolní části zaškrtněte políčko **Zobrazit heslo** , a poznamenejte si automaticky vygenerované heslo.
4. Výběrem **vytvořit** uložte nového uživatele.

E-mailová adresa uživatelského účtu musí být ověřený název domény v adresáři. Všechny ověřené domény v adresáři můžete zobrazit tak, že v nabídce vlevo vyberete **Azure Active Directory**  ->  **vlastní názvy domén** .

Další informace o přidávání vlastních domén v Azure Active Directory najdete v tématu [Přidání nebo přidružení domény ve službě Azure AD](../../active-directory/active-directory-add-domain.md).

## <a name="troubleshoot-work-email-sign-in"></a>Řešení potíží s přihlášením k pracovnímu e-mailu

Pokud máte potíže s přihlášením ke svému pracovnímu účtu (známému taky jako tenant Azure AD), najděte scénář na diagramu níže, který nejlépe odpovídá vaší situaci, a postupujte podle doporučených kroků.

![Diagram pro řešení potíží s přihlášením k pracovnímu účtu](./media/onboarding-aad-flow.png)

## <a name="next-steps"></a>Další kroky

- [Správa účtu komerčního tržiště v partnerském centru](./manage-account.md) 
