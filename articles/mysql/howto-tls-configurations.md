---
title: Konfigurace TLS-Azure Portal-Azure Database for MySQL
description: Naučte se nastavit konfiguraci TLS pomocí Azure Portal pro Azure Database for MySQL
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: 46eaa6a3b97967da9c4743d0cf1f6edc8f90b1ce
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119780"
---
# <a name="configuring-tls-settings-in-azure-database-for-mysql-using-azure-portal"></a>Konfigurace nastavení TLS v Azure Database for MySQL pomocí Azure Portal

Tento článek popisuje, jak můžete nakonfigurovat server Azure Database for MySQL, aby vynutil minimální verzi TLS povolenou pro připojení přes a odepřít všechna připojení s nižší verzí TLS, než je nakonfigurovaná minimální verze protokolu TLS, a tím zvýšit zabezpečení sítě.

Verzi TLS můžete vynutili pro připojení k jejich Azure Database for MySQL. Zákazníci teď mají možnost nastavit minimální verzi protokolu TLS pro svůj databázový server. Například nastavení minimální verze protokolu TLS na 1,0 znamená, že umožníte klientům připojit se pomocí TLS 1.0, 1.1 a 1,2. Případně můžete nastavení nastavit na 1,2 znamená, že povolíte jenom klienty připojující se pomocí protokolu TLS 1.2 + a všechna příchozí připojení s TLS 1,0 a TLS 1,1 budou odmítnutá.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto průvodce budete potřebovat:

* [Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mysql"></a>Nastavit konfigurace TLS pro Azure Database for MySQL

Pomocí těchto kroků nastavte minimální verzi TLS serveru MySQL:

1. V [Azure Portal](https://portal.azure.com/)vyberte svůj existující Azure Database for MySQL server.

1. Na stránce serveru MySQL v části **Nastavení**klikněte na **zabezpečení připojení** . tím otevřete stránku konfigurace zabezpečení připojení.

1. V **minimální verzi protokolu TLS**vyberte **1,2** pro zamítnutí připojení s protokolem TLS nižším než TLS 1,2 pro váš server MySQL.

    ![Konfigurace Azure Database for MySQL TLS](./media/howto-tls-configurations/setting-tls-value.png)

1. Kliknutím na **Uložit** uložte změny.

1. Oznámení ověří, že nastavení zabezpečení připojení bylo úspěšně povoleno.

    ![Úspěšná konfigurace TLS Azure Database for MySQL](./media/howto-tls-configurations/setting-tls-value-success.png)

## <a name="next-steps"></a>Další kroky

- Informace o [tom, jak vytvářet výstrahy na metrikách](howto-alert-on-metric.md)