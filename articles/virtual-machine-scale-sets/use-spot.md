---
title: Vytvoření sady škálování, která používá virtuální počítače Azure
description: Naučte se vytvářet služby Azure Virtual Machine Scale Sets, které k úsporám šetří náklady pomocí virtuálních počítačů na místě.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: spot
ms.date: 03/25/2020
ms.reviewer: jagaveer
ms.custom: jagaveer
ms.openlocfilehash: 70d7eb000ed2d50bc22bb005621ee7515e5a2a61
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86527451"
---
# <a name="azure-spot-vms-for-virtual-machine-scale-sets"></a>Virtuální počítače Azure na místě pro Virtual Machine Scale Sets 

Používání Azure na základě služby škálování na úrovni služby umožňuje využít výhod naší nevyužité kapacity s významnou úsporou nákladů. V jakémkoli okamžiku, kdy Azure potřebuje kapacitu zpět, bude infrastruktura Azure vyřadit instance v přímých intervalech. Proto jsou významné instance pro úlohy, které mohou zpracovávat přerušení jako úlohy dávkového zpracování, vývojová a testovací prostředí, velké výpočetní úlohy a další, velmi Skvělé.

Množství dostupné kapacity se může lišit v závislosti na velikosti, oblasti, denní době a dalších. Při nasazování přímých instancí na sady škálování Azure přidělí tuto instanci jenom v případě, že je dostupná kapacita, ale pro tyto instance neexistuje žádná smlouva SLA. Sada škálování na místě je nasazená v jedné doméně selhání a neposkytuje žádné záruky vysoké dostupnosti.


## <a name="pricing"></a>Ceny

Ceny pro instance přímých instancí jsou proměnné na základě oblastí a SKU. Další informace najdete v tématu ceny pro [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) a [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/). 


S proměnnými cenami máte možnost nastavit maximální cenu v USD (USD), která používá až 5 desetinných míst. Hodnota by měla být například `0.98765` maximální cena $0,98765 USD za hodinu. Pokud nastavíte maximální cenu, instance se nevyřadí na `-1` základě ceny. Cena za instanci bude aktuální cena za cenu nebo cena standardní instance, která je stále menší, pokud je k dispozici kapacita a kvóta.

## <a name="eviction-policy"></a>Zásady vyřazení

Při vytváření sad s přímým škálováním můžete nastavit zásadu vyřazení, aby se nastavilo zrušení *přidělení* (výchozí) nebo *odstranění*. 

Zásady zrušení *přidělení* přesouvá vaše vyřazené instance do stavu Zastaveno (přidělení zrušeno), což vám umožní znovu nasadit vyřazené instance. Neexistuje však záruka, že přidělení bude úspěšné. Navrácené virtuální počítače se budou počítat s kvótou instance sady škálování a budou se vám účtovat vaše základní disky. 

Pokud chcete, aby se vaše instance na škále vašich přímých škálování odstranily při jejich vyřazení, můžete nastavit zásadu vyřazení, která se má *Odstranit*. Když je zásada vyřazení nastavená tak, aby se odstranila, můžete vytvořit nové virtuální počítače tím, že zvýšíte vlastnost počet instancí sady škálování. Vyřazení virtuálních počítačů se odstraní společně s jejich podkladovým diskům, takže se za úložiště nebudete účtovat. K automatickému vyzkoušení a kompenzaci vydaných virtuálních počítačů můžete použít také funkci automatického škálování sad škálování, ale nezaručujeme, že přidělení bude úspěšné. Pokud nastavíte zásadu vyřazení na hodnotu odstranit, doporučujeme vám používat jenom funkci automatického škálování na škále bodů obnovení, abyste se vyhnuli nákladům na vaše disky a omezeními kvót. 

Uživatelé se můžou přihlásit k přijímání oznámení v rámci virtuálního počítače prostřednictvím [Azure Scheduled Events](../virtual-machines/linux/scheduled-events.md). To vám upozorní na to, jestli se virtuální počítače vyloučí a že budete mít 30 sekund na dokončení všech úloh a před vyřazením provést úlohy vypnutí. 


## <a name="deploying-spot-vms-in-scale-sets"></a>Nasazení virtuálních počítačů na místě v sadách škálování

Pokud chcete nasadit virtuální počítače na místě v sadě škálování, můžete nastavit příznak nové *priority* tak, aby byl *bodový*. Všechny virtuální počítače ve vaší sadě škálování budou nastavené na bodové. Pokud chcete vytvořit sadu škálování s virtuálními počítači, použijte jednu z následujících metod:
- [Azure Portal](#portal)
- [Azure CLI](#azure-cli)
- [Azure PowerShell](#powershell)
- [Šablony Azure Resource Manager](#resource-manager-templates)

## <a name="portal"></a>Portál

Proces vytvoření sady škálování, která používá virtuální počítače na místě, je stejný, jak je popsáno v [článku Začínáme](quick-create-portal.md). Když nasazujete sadu škálování, můžete nastavit příznak bodu a zásadu vyřazení: ![ Vytvoření sady škálování s virtuálními počítači s přímým použitím virtuálních počítačů.](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-max-price.png)


## <a name="azure-cli"></a>Azure CLI

Proces vytvoření sady škálování se stejnými virtuálními počítači je stejný, jak je popsáno v [článku Začínáme](quick-create-cli.md). Stačí přidat klíčové slovo--priority a přidat `--max-price` . V tomto příkladu používáme `-1` pro, `--max-price` takže instance nebude vyřazení na základě ceny.

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 
```

## <a name="powershell"></a>PowerShell

Proces vytvoření sady škálování se stejnými virtuálními počítači je stejný, jak je popsáno v [článku Začínáme](quick-create-powershell.md).
Stačí přidat klíčové slovo "– prioritní" a zadat `-max-price` do příkazu [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig).

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Spot" `
    --max-price -1
```

## <a name="resource-manager-templates"></a>Šablony Resource Manageru

Proces vytvoření sady škálování, která používá bodové virtuální počítače, je stejný, jak je popsáno v článku Začínáme pro [Linux](quick-create-template-linux.md) nebo [Windows](quick-create-template-windows.md). 

Pro nasazení šablon přímých `"apiVersion": "2019-03-01"` verzí použijte nebo novější. Přidejte do `priority` `evictionPolicy` `billingProfile` `"virtualMachineProfile":` části šablony a vlastnosti: 

```json
                "priority": "Spot",
                "evictionPolicy": "Deallocate",
                "billingProfile": {
                    "maxPrice": -1
                }
```

Chcete-li odstranit instanci poté, co byla vyřazena, změňte `evictionPolicy` parametr na `Delete` .

## <a name="faq"></a>Nejčastější dotazy

**Otázka:** Po vytvoření je stejná jako instance stejné jako standardní instance?

**A:** Ano, s výjimkou smlouvy SLA pro virtuální počítače na místě a jejich vyřazení z provozu kdykoli se dá provést.


**Otázka:** Co dělat při vyřazení, ale stále potřebují kapacitu?

**A:** Pokud potřebujete kapacitu hned, doporučujeme použít virtuální počítače místo přímých virtuálních počítačů.


**Otázka:** Jak se Správa kvót spravuje pro místo?

**A:** Instance bodů a standardní instance budou mít samostatné fondy kvót. Kvóta na místě se bude sdílet mezi virtuálními počítači a instancemi sady škálování. Další informace najdete v tématu [Limity, kvóty a omezení předplatného a služeb Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).


**Otázka:** Můžu požádat o další kvótu na místě?

**A:** Ano, žádost budete moci odeslat, abyste zvýšili kvótu pro virtuální počítače pomocí [procesu žádosti o standardní kvótu](../azure-portal/supportability/per-vm-quota-requests.md).


**Otázka:** Můžu převést existující sady škálování na škálované sady škálování?

**A:** Ne, nastavení `Spot` příznaku se podporuje jenom při vytvoření.


**Otázka:** Pokud používám `low` sadu škálování s nízkou prioritou, musím `Spot` místo toho začít používat?

**A:** Prozatím `low` `Spot` bude fungovat i, ale měli byste začít s přechodem na použití `Spot` .


**Otázka:** Můžu vytvořit sadu škálování s pravidelnými virtuálními počítači i s virtuálními počítači?

**A:** Ne, sada škálování nepodporuje více než jeden typ priority.


**Otázka:**  Můžu používat automatické škálování se sadami škálování na místě?

**A:** Ano, můžete nastavit pravidla automatického škálování pro sadu škálování na místě. Pokud jsou vaše virtuální počítače vyřazené, automatické škálování se může pokusit vytvořit nové virtuální počítače na místě. Nezapomeňte, že tuto kapacitu nezaručujete. 


**Otázka:**  Funguje automatické škálování podle zásad vyřazení (navrácení a odstranění)?

**A:** Doporučuje se nastavit zásadu vyřazení, která se má odstranit při použití automatického škálování. Důvodem je to, že nepřidělené instance se počítají na základě počtu kapacit v sadě škálování. Při použití automatického škálování se pravděpodobně vám v důsledku navrácených instancí dokončí počet cílových instancí rychleji. 


**Otázka:** Jaké kanály podporují přímé virtuální počítače?

**A:** V následující tabulce najdete informace o dostupnosti virtuálních počítačů.

<a name="channel"></a>

| Kanály Azure               | Dostupnost virtuálních počítačů Azure       |
|------------------------------|-----------------------------------|
| Smlouva Enterprise         | Ano                               |
| Pay As You Go                | Ano                               |
| Poskytovatel cloudových služeb (CSP) | [Obraťte se na svého partnera.](/partner-center/azure-plan-get-started) |
| Výhody                     | Není k dispozici                     |
| Financovan                    | Ano                               |
| Bezplatná zkušební verze                   | Není k dispozici                     |


**Otázka:** Kde můžu publikovat otázky?

**A:** Svůj dotaz můžete odeslat a označit `azure-spot` na adrese [Q&A](/answers/topics/azure-spot.html). 

## <a name="next-steps"></a>Další kroky

Podrobnosti o cenách najdete na [stránce s cenami sady škálování virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) .
