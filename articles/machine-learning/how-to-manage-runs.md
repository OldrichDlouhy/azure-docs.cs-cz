---
title: Spuštění, monitorování a zrušení školicích běhů v Pythonu
titleSuffix: Azure Machine Learning
description: Naučte se, jak začít, nastavit stav, označit a uspořádat experimenty pro strojové učení.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 01/09/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: 38e7ecf371beb003137b4ae1b2315046a474d090
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319724"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Spuštění, monitorování a zrušení školicích běhů v Pythonu
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

[Sada SDK Azure Machine Learning pro Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), [Machine Learning CLI](reference-azure-machine-learning-cli.md)a [Azure Machine Learning Studio](https://ml.azure.com) nabízí různé metody pro monitorování, organizování a správu vašich běhů pro školení a experimentování.

Tento článek ukazuje příklady následujících úloh:

* Monitorujte výkon spouštění.
* Zrušení nebo selhání spuštění.
* Vytvoření podřízených spuštění.
* Označení a hledání spuštění.

## <a name="prerequisites"></a>Požadavky

Budete potřebovat následující položky:

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, vytvořte si napřed bezplatný účet. Vyzkoušení [bezplatné nebo placené verze Azure Machine Learning](https://aka.ms/AMLFree) dnes

* [Pracovní prostor Azure Machine Learning](how-to-manage-workspace.md).

* Sada SDK Azure Machine Learning pro Python (verze 1.0.21 nebo novější). Chcete-li nainstalovat nebo aktualizovat nejnovější verzi sady SDK, přečtěte si téma [instalace nebo aktualizace sady SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Chcete-li zjistit verzi sady Azure Machine Learning SDK, použijte následující kód:

    ```python
    print(azureml.core.VERSION)
    ```

* Rozšíření [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) a rozhraní příkazového [řádku pro Azure Machine Learning](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Spuštění běhu a jeho procesu protokolování

### <a name="using-the-sdk"></a>Použití sady SDK

Nastavte experiment tak, že naimportujete třídy [pracovní prostor](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)a [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) z balíčku [AzureML. Core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) .

```python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Spusťte běh a jeho proces protokolování s [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) metodou.

```python
notebook_run = exp.start_logging()
notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>Použití rozhraní příkazového řádku

Pro spuštění experimentu použijte následující postup:

1. V prostředí nebo příkazovém řádku můžete pomocí Azure CLI ověřit předplatné Azure:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

1. Připojte konfiguraci pracovního prostoru ke složce, která obsahuje váš školicí skript. Nahraďte `myworkspace` pracovním prostorem Azure Machine Learning. Nahraďte `myresourcegroup` skupinou prostředků Azure, která obsahuje váš pracovní prostor:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Tento příkaz vytvoří `.azureml` podadresář, který obsahuje příklady souborů prostředí RunConfig a conda. Obsahuje taky `config.json` soubor, který se používá ke komunikaci s vaším pracovním prostorem Azure Machine Learning.

    Další informace najdete v tématu [AZ ml složka připojit](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Chcete-li spustit spuštění, použijte následující příkaz. Při použití tohoto příkazu zadejte název souboru RunConfig (text před \* . RunConfig, pokud hledáte v systému souborů) s parametrem-c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > `az ml folder attach`Příkaz vytvořil `.azureml` podadresář, který obsahuje dva příklady souborů RunConfig.
    >
    > Pokud máte skript Pythonu, který vytvoří objekt konfigurace spuštění programově, můžete použít [RunConfig. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) a uložit ho jako soubor RunConfig.
    >
    > Další příklady souborů RunConfig naleznete v tématu [https://github.com/MicrosoftDocs/pipelines-azureml/](https://github.com/MicrosoftDocs/pipelines-azureml/) .

    Další informace najdete v tématu [AZ ml Run odeslání-Script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

### <a name="using-azure-machine-learning-studio"></a>Používání Azure Machine Learning studia

Pokud chcete spustit odeslání kanálu v Návrháři (Preview), použijte následující postup:

1. Nastavte pro svůj kanál výchozí výpočetní cíl.

1. V horní části plátna kanálu vyberte **Spustit** .

1. Vyberte experiment pro seskupení spuštění kanálu.

## <a name="monitor-the-status-of-a-run"></a>Monitoruje stav spuštění.

### <a name="using-the-sdk"></a>Použití sady SDK

Získejte stav spuštění s [`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) metodou.

```python
print(notebook_run.get_status())
```

Chcete-li získat ID spuštění, dobu provádění a další podrobnosti o spuštění, použijte [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) metodu.

```python
print(notebook_run.get_details())
```

Po úspěšném dokončení spuštění použijte [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) metodu k označení jako dokončenou.

```python
notebook_run.complete()
print(notebook_run.get_status())
```

Použijete `with...as` -li vzor návrhu Pythonu, bude spuštění při nedostatku rozsahu automaticky označen jako dokončené. Nemusíte ručně označit rutinu Run jako dokončenou.

```python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print(notebook_run.get_status())

print(notebook_run.get_status())
```

### <a name="using-the-cli"></a>Použití rozhraní příkazového řádku

1. Chcete-li zobrazit seznam spuštění experimentu, použijte následující příkaz. Nahraďte `experiment` názvem experimentu:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Tento příkaz vrátí dokument JSON se seznamem informací o běhu pro tento experiment.

    Další informace najdete v tématu [AZ ml experiment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Chcete-li zobrazit informace o konkrétním spuštění, použijte následující příkaz. Nahraďte `runid` ID běhu:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Tento příkaz vrátí dokument JSON se seznamem informací o běhu.

    Další informace najdete v tématu [AZ ml Run show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).


### <a name="using-azure-machine-learning-studio"></a>Používání Azure Machine Learning studia

Pro zobrazení počtu aktivních běhů experimentu v studiu.

1. Přejděte do části **experimenty** ... 

1. Vyberte experiment.

    Na stránce experiment můžete zobrazit počet aktivních cílových výpočtů a dobu trvání každého spuštění. 

1. Vyberte konkrétní číslo spuštění.

1. Na kartě **protokoly** můžete najít diagnostické a chybové protokoly pro spuštění kanálu.


## <a name="cancel-or-fail-runs"></a>Zrušení nebo selhání spuštění

Pokud si všimnete omylem nebo pokud dokončení běhu trvá příliš dlouho, můžete spustit operaci.

### <a name="using-the-sdk"></a>Použití sady SDK

Chcete-li zrušit běh pomocí sady SDK, použijte [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) metodu:

```python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_script_run = exp.submit(run_config)
print(local_script_run.get_status())

local_script_run.cancel()
print(local_script_run.get_status())
```

Pokud se vaše spuštění dokončí, ale obsahuje chybu (například byl použit nesprávný školicí skript), můžete použít [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) metodu k jejímu označení jako neúspěšné.

```python
local_script_run = exp.submit(run_config)
local_script_run.fail()
print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>Použití rozhraní příkazového řádku

Chcete-li zrušit běh pomocí rozhraní příkazového řádku, použijte následující příkaz. Nahraďte `runid` ID běhu.

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Další informace najdete v tématu [AZ ml Run Cancel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

### <a name="using-azure-machine-learning-studio"></a>Používání Azure Machine Learning studia

Chcete-li zrušit spuštění v nástroji Studio, použijte následující postup:

1. V části **experimenty** nebo **kanály** se dostanete na běžící kanál. 

1. Vyberte číslo běhu kanálu, které chcete zrušit.

1. Na panelu nástrojů vyberte **Zrušit** .


## <a name="create-child-runs"></a>Vytvořit podřízená spuštění

Vytvoření podřízených běhů pro seskupení souvisejících běhů, například pro různé iterace ladění parametrů.

> [!NOTE]
> Podřízená spuštění lze vytvořit pouze pomocí sady SDK.

Tento příklad kódu používá `hello_with_children.py` skript k vytvoření dávky pěti podřízených spuštění v rámci odeslaného běhu pomocí [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) metody:

```python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Při přesunu z oboru jsou podřízená spuštění automaticky označena jako dokončená.

Chcete-li vytvořit mnoho podřízených spuštění efektivně, použijte [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) metodu. Vzhledem k tomu, že při každém vytváření dojde k síťovému volání, vytvoření dávky spuštění je efektivnější než jejich vytvoření po jednom.

### <a name="submit-child-runs"></a>Odeslat podřízená spuštění

Podřízené běhy lze také odeslat z nadřazeného spuštění. To umožňuje vytvářet hierarchie nadřazených a podřízených spuštění. 

Můžete chtít, aby vaše podřízená spuštění používala jinou konfiguraci spuštění než v nadřazeném spuštění. Můžete například použít méně náročnou konfiguraci založenou na PROCESORech pro nadřazený objekt při použití konfigurací založených na GPU pro vaše děti. Dalším běžným přáním je předat každý podřízený objekt různým argumentům a datům. Chcete-li přizpůsobit podřízený běh, předejte `RunConfiguration` objekt konstruktoru podřízeného objektu `ScriptRunConfig` . Tento příklad kódu, který by byl součástí `ScriptRunConfig` skriptu nadřazeného objektu:

- Vytvoří `RunConfiguration` načtení pojmenovaného výpočetního prostředku.`"gpu-compute"`
- Iteruje přes různé hodnoty argumentů, které se mají předat podřízeným `ScriptRunConfig` objektům.
- Vytvoří a odešle nové podřízené spuštění pomocí vlastního výpočetního prostředku a argumentu.
- Blokuje až do dokončení všech podřízených spuštění.

```python
# parent.py
# This script controls the launching of child scripts
from azureml.core import Run, ScriptRunConfig, RunConfiguration

run_config_for_aml_compute = RunConfiguration()
run_config_for_aml_compute.target = "gpu-compute"
run_config_for_aml_compute.environment.docker.enabled = True 

run = Run.get_context()

child_args = ['Apple', 'Banana', 'Orange']
for arg in child_args: 
    run.log('Status', f'Launching {arg}')
    child_config = ScriptRunConfig(source_directory=".", script='child.py', arguments=['--fruit', arg], run_config = run_config_for_aml_compute)
    # Starts the run asynchronously
    run.submit_child(child_config)

# Experiment will "complete" successfully at this point. 
# Instead of returning immediately, block until child runs complete

for child in run.get_children():
    child.wait_for_completion()
```

Chcete-li vytvořit mnoho podřízených spuštění se stejnou konfigurací, argumenty a vstupy efektivně, použijte [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) metodu. Vzhledem k tomu, že při každém vytváření dojde k síťovému volání, vytvoření dávky spuštění je efektivnější než jejich vytvoření po jednom.

V rámci podřízeného spuštění můžete zobrazit ID nadřazeného spuštění:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Dotaz na podřízená spuštění

Chcete-li zadat dotaz na podřízená spuštění konkrétního nadřazeného objektu, použijte [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) metodu. ``recursive = True``Argument slouží k dotazování vnořené stromové struktury podřízených a podřízené.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Označení a hledání spuštění

V Azure Machine Learning můžete použít vlastnosti a značky, které vám pomůžou organizovat a dotazovat vaše běhy na důležité informace.

### <a name="add-properties-and-tags"></a>Přidat vlastnosti a značky

#### <a name="using-the-sdk"></a>Použití sady SDK

Chcete-li přidat hledaná metadata k vašim běhům, použijte [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) metodu. Například následující kód přidá `"author"` vlastnost do běhu:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Vlastnosti jsou neměnné, takže vytvoří trvalý záznam pro účely auditování. Následující příklad kódu má za následek chybu, protože jsme už přidali `"azureml-user"` jako `"author"` hodnotu vlastnosti v předchozím kódu:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Na rozdíl od vlastností jsou značky proměnlivé. K přidání prohledávatelných a smysluplných informací pro uživatele experimentu použijte [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) metodu.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Můžete také přidat jednoduché řetězcové značky. Když se tyto značky objeví ve slovníku značek jako klíče, mají hodnotu `None` .

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>Použití rozhraní příkazového řádku

> [!NOTE]
> Pomocí rozhraní příkazového řádku můžete přidat nebo aktualizovat pouze značky.

Chcete-li přidat nebo aktualizovat značku, použijte následující příkaz:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Další informace najdete v tématu [AZ ml Run Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Vlastnosti a značky dotazu

Dotazování můžete spustit v experimentu a vrátit seznam spuštění, který odpovídá specifickým vlastnostem a značkám.

#### <a name="using-the-sdk"></a>Použití sady SDK

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>Použití rozhraní příkazového řádku

Rozhraní příkazového řádku Azure podporuje dotazy [JMESPath](http://jmespath.org) , které lze použít k filtrování spuštění na základě vlastností a značek. Pokud chcete použít dotaz JMESPath s rozhraním příkazového řádku Azure CLI, zadejte ho s `--query` parametrem. Následující příklady znázorňují základní dotazy pomocí vlastností a značek:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Další informace o dotazování na výsledky rozhraní příkazového řádku Azure najdete v tématu [dotazování výstupu příkazu Azure CLI](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

### <a name="using-azure-machine-learning-studio"></a>Používání Azure Machine Learning studia

1. Přejděte do části **kanály** .

1. Pomocí panelu hledání můžete filtrovat kanály pomocí značek, popisů, názvů experimentů a jména odesílatele.

## <a name="example-notebooks"></a>Příklady poznámkových bloků

Následující poznámkové bloky ukazují koncepty v tomto článku:

* Další informace o rozhraních API pro protokolování najdete v [poznámkovém bloku rozhraní API pro protokolování](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Další informace o správě běžících v sadě Azure Machine Learning SDK najdete v [poznámkovém bloku spravovat běhy](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Další kroky

* Informace o tom, jak zaznamenat metriky pro vaše experimenty, najdete v tématu [metriky protokolu během školicích běhů](how-to-track-experiments.md).
* Informace o tom, jak monitorovat prostředky a protokoly z Azure Machine Learning, najdete v tématu [monitorování Azure Machine Learning](monitor-azure-machine-learning.md).
