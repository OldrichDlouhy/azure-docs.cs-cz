---
title: Co je výpočetní instance služby Azure Machine Learning?
titleSuffix: Azure Machine Learning
description: Přečtěte si o Azure Machine Learning výpočetní instanci, plně spravované cloudové pracovní stanici.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 07/27/2020
ms.openlocfilehash: 4ac95fa81fdbee237cacaa1541e333bb70c370fa
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323294"
---
# <a name="what-is-an-azure-machine-learning-compute-instance"></a>Co je výpočetní instance služby Azure Machine Learning?

Instance služby Azure Machine Learning COMPUTE je spravovaná cloudová pracovní stanice pro odborníky na data.

Výpočetní instance usnadňují začátek vývoje Azure Machine Learning a také poskytují možnosti pro správu a připravenost pro správce IT.  

Použijte výpočetní instanci jako vaše plně nakonfigurované a spravované vývojové prostředí v cloudu pro strojové učení. Můžou se taky používat jako výpočetní cíl pro školení a Inferencing pro účely vývoje a testování.  

Pro vzdělávání modelů produkčního prostředí použijte [Azure Machine Learning výpočetní cluster](how-to-set-up-training-targets.md#amlcompute) s možnostmi škálování s více uzly. Pro nasazení modelu produkčního prostředí použijte [cluster služby Azure Kubernetes](how-to-deploy-azure-kubernetes-service.md).

## <a name="why-use-a-compute-instance"></a>Proč používat výpočetní instanci?

Výpočetní instance je plně spravovaná cloudová pracovní stanice optimalizovaná pro vývojové prostředí ve službě Machine Learning. Přináší následující výhody:

|Klíčové výhody|Popis|
|----|----|
|Produktivita|Modely můžete vytvářet a nasazovat pomocí integrovaných poznámkových bloků a následujících nástrojů v Azure Machine Learning Studiu:<br/>– Jupyter<br/>- JupyterLab<br/>-RStudio (Preview)<br/>Instance COMPUTE je plně integrovaná do Azure Machine Learningho pracovního prostoru a studia. Poznámkové bloky a data můžete sdílet s dalšími odborníky na data v pracovním prostoru. Můžete také nastavit VS Code vzdáleného vývoje pomocí [SSH](how-to-set-up-vs-code-remote.md) . |
|Spravované & zabezpečené|Snižte nároky na zabezpečení a přidejte dodržování požadavků podnikového zabezpečení. Výpočetní instance poskytují robustní zásady správy a zabezpečené síťové konfigurace, jako jsou:<br/><br/>– Automatické zřizování z Správce prostředků šablon nebo Azure Machine Learning SDK<br/>- [Řízení přístupu na základě role (RBAC)](/azure/role-based-access-control/overview)<br/>- [Podpora virtuální sítě](how-to-enable-virtual-network.md#compute-instance)<br/>-Zásada SSH pro povolení nebo zakázání přístupu SSH<br/>Protokol TLS 1,2 povolen |
|Předem nakonfigurovaná &nbsp; pro &nbsp; ml|Ušetřete čas při instalaci s předem nakonfigurovanými a aktuálními balíčky ML, architekturou pro hloubkové učení a ovladači GPU.|
|Plně přizpůsobitelné|Široká podpora typů virtuálních počítačů Azure, včetně GPU a trvalého přizpůsobení nízké úrovně, jako je instalace balíčků a ovladačů, vede k pokročilým scénářům Breeze. |

## <a name="tools-and-environments"></a><a name="contents"></a>Nástroje a prostředí

> [!IMPORTANT]
> Nástroje označené (Preview) jsou momentálně ve verzi Public Preview.
> Verze Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro produkční úlohy. Některé funkce se nemusí podporovat nebo mohou mít omezené možnosti. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Instance Azure Machine Learning COMPUTE vám umožní vytvářet, vyškolovat a nasazovat modely v plně integrovaném prostředí poznámkového bloku v pracovním prostoru.

Tyto nástroje a prostředí se nainstalují do výpočetní instance: 

|Obecné nástroje & prostředí|Podrobnosti|
|----|:----:|
|Ovladače|`CUDA`</br>`cuDNN`</br>`NVIDIA`</br>`Blob FUSE` |
|Knihovna Intel MPI||
|Azure CLI ||
|Ukázky Azure Machine Learning ||
|Docker||
|Nginx||
|NCCL 2,0 ||
|Protobuf|| 

|Prostředí pro & nástrojů **R**|Podrobnosti|
|----|:----:|
|RStudio server Open Source Edition (Preview)||
|Jádro R||
|Sada SDK Azure Machine Learning pro R|[azuremlsdk](https://azure.github.io/azureml-sdk-for-r/reference/index.html)</br>Ukázky sady SDK|

|Prostředí & **Python** Tools|Podrobnosti|
|----|----|
|Anaconda Python||
|Jupyter a rozšíření||
|Jupyterlab a rozšíření||
[Sada Azure Machine Learning SDK pro Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)</br>z PyPI|Zahrnuje většinu dalších balíčků AzureML.  Pokud chcete zobrazit úplný seznam, [otevřete okno terminálu na instanci služby COMPUTE](how-to-run-jupyter-notebooks.md#terminal) a spusťte příkaz. <br/> `conda list -n azureml_py36 azureml*` |
|Další balíčky PyPI|`jupytext`</br>`tensorboard`</br>`nbconvert`</br>`notebook`</br>`Pillow`|
|Balíčky conda|`cython`</br>`numpy`</br>`ipykernel`</br>`scikit-learn`</br>`matplotlib`</br>`tqdm`</br>`joblib`</br>`nodejs`</br>`nb_conda_kernels`|
|Balíčky pro hloubkové učení|`PyTorch`</br>`TensorFlow`</br>`Keras`</br>`Horovod`</br>`MLFlow`</br>`pandas-ml`</br>`scrapbook`|
|Balíčky ONNX|`keras2onnx`</br>`onnx`</br>`onnxconverter-common`</br>`skl2onnx`</br>`onnxmltools`|
|Ukázky Azure Machine Learning Python & R SDK||

Balíčky Pythonu jsou nainstalované v prostředí **python 3,6-AzureML** .  

### <a name="installing-packages"></a>Instalace balíčků

Balíčky můžete nainstalovat přímo do poznámkového bloku Jupyter nebo RStudio:

* RStudio použijte kartu **balíčky** v pravém dolním rohu nebo kartu **Konzola** v levém horním rohu.  
* Python: přidejte instalační kód a spusťte ho v buňce Jupyter poznámkového bloku.

Případně můžete k oknu terminálu přistupovat některým z těchto způsobů:

* RStudio: v levém horním rohu vyberte kartu **terminálu** .
* Jupyter Lab: v **druhém** záhlaví karty spouštěče vyberte dlaždici **terminálu** .
* Jupyter: v pravém horním rohu na kartě soubory vyberte **nový>terminálu** .
* SSH k počítači  Pak nainstalujte balíčky Pythonu do prostředí **Python 3,6-AzureML** .  Nainstalujte balíčky R do prostředí jazyka **r** .

## <a name="accessing-files"></a>Přístup k souborům

Poznámkové bloky a skripty jazyka R se ukládají ve výchozím účtu úložiště vašeho pracovního prostoru ve sdílené souborové službě Azure.  Tyto soubory jsou umístěny v adresáři "uživatelské soubory". Toto úložiště usnadňuje sdílení poznámkových bloků mezi výpočetními instancemi. Účet úložiště také udržuje při zastavení nebo odstranění výpočetní instance bezpečně zachované vaše poznámkové bloky.

Účet sdílené složky Azure vašeho pracovního prostoru je připojen jako jednotka na výpočetní instanci. Tato jednotka je výchozím pracovním adresářem pro Jupyter, Jupyter Labs a RStudio. To znamená, že se poznámkové bloky a jiné soubory, které vytvoříte v Jupyter, JupyterLab nebo RStudio, automaticky ukládají do sdílené složky a jsou dostupné pro použití v jiných výpočetních instancích.

Soubory ve sdílené složce jsou dostupné ze všech výpočetních instancí ve stejném pracovním prostoru. Všechny změny těchto souborů v instanci COMPUTE budou spolehlivě trvale zachované zpátky do sdílené složky.

Nejnovější ukázky Azure Machine Learning můžete také klonovat do složky v adresáři uživatelských souborů ve sdílené složce pracovního prostoru.

Zápis malých souborů může být pomalejší na síťových jednotkách, než je zapisuje na samotný místní disk výpočetní instance.  Pokud píšete mnoho malých souborů, zkuste použít adresář přímo na výpočetní instanci, jako je například `/tmp` adresář. Upozorňujeme prosím, že tyto soubory nebudou dostupné z jiných výpočetních instancí. 

`/tmp`Pro dočasná data můžete použít adresář v instanci služby Compute.  Nepište ale velké soubory dat na disk s operačním systémem výpočetní instance.  Místo toho použijte [úložiště dat](concept-azure-machine-learning-architecture.md#datasets-and-datastores) . Pokud jste nainstalovali rozšíření Git JupyterLab, může také vést k zpomalení výkonu výpočetní instance.

## <a name="managing-a-compute-instance"></a>Správa výpočetní instance

V pracovním prostoru v Azure Machine Learning Studiu vyberte **COMPUTE**a pak v horní části vyberte **výpočetní instanci** .

![Správa výpočetní instance](./media/concept-compute-instance/manage-compute-instance.png)

Můžete provést následující akce:

* [Vytvořte výpočetní instanci](#create). 
* Aktualizujte kartu COMPUTE Instances.
* Spusťte, zastavte a restartujte výpočetní instanci.  Za instanci platíte za každé, když je spuštěná. Pokud nepoužíváte výpočetní instanci, můžete ji zastavit, abyste snížili náklady. Zastavení výpočetní instance ho zruší. Pak ho znovu spusťte, až ho budete potřebovat. 
* Odstraňte výpočetní instanci.
* Vyfiltrujte seznam výpočetních instancí na ty, které jste vytvořili.  Jedná se o výpočetní instance, ke kterým máte přístup.

Pro každou výpočetní instanci ve vašem pracovním prostoru, ke kterému máte přístup, můžete:

* Přístup k Jupyter, JupyterLab, RStudio instance COMPUTE
* SSH do výpočetní instance. Přístup SSH je ve výchozím nastavení zakázán, ale lze jej povolit v době vytváření výpočetních instancí. Přístup přes SSH je prostřednictvím mechanismu veřejného a privátního klíče. Karta vám poskytne podrobnosti o připojení SSH, jako je například IP adresa, uživatelské jméno a číslo portu.
* Získejte podrobnosti o konkrétní výpočetní instanci, jako je třeba IP adresa a oblast.

[RBAC](/azure/role-based-access-control/overview) umožňuje řídit, kteří uživatelé v pracovním prostoru můžou vytvořit, odstranit, spustit, zastavit a restartovat výpočetní instanci. Všichni uživatelé v roli přispěvatel a vlastník pracovního prostoru můžou vytvářet, odstraňovat, spouštět, zastavovat a restartovat výpočetní instance v rámci pracovního prostoru. Pouze tvůrce konkrétní výpočetní instance má však povolen přístup k Jupyter, JupyterLab a RStudio této výpočetní instance. Tvůrce výpočetní instance má vyhrazenou instanci COMPUTE, má root Access a může být terminálem prostřednictvím Jupyter/JupyterLab/RStudio. Instance COMPUTE bude mít přihlášení uživatele Creator User a všechny akce budou používat identitu tohoto uživatele pro RBAC a navýšení spuštění experimentů. Přístup přes SSH je řízený pomocí mechanismu veřejného a privátního klíče.

Tyto akce lze řídit pomocí RBAC:
* *Microsoft. MachineLearningServices/pracovní prostory/výpočetní výkon/čtení*
* *Microsoft. MachineLearningServices/pracovní prostory/výpočty/zapisovat*
* *Microsoft. MachineLearningServices/pracovní prostory/výpočty/odstranit*
* *Microsoft. MachineLearningServices/pracovní prostory/výpočty/spustit/akce*
* *Microsoft. MachineLearningServices/pracovní prostory/výpočty/zastavit/akce*
* *Microsoft. MachineLearningServices/pracovní prostory/výpočty/restartovat/akce*

### <a name="create-a-compute-instance"></a><a name="create"></a>Vytvoření výpočetní instance

Pokud jste připraveni spustit jeden z vašich poznámkových bloků, vytvořte v pracovním prostoru v Azure Machine Learning Studiu novou instanci služby COMPUTE z oddílu **COMPUTE** nebo v části **poznámkové bloky** .

:::image type="content" source="media/concept-compute-instance/create-compute-instance.png" alt-text="Vytvořit novou výpočetní instanci":::


|Pole  |Popis  |
|---------|---------|
|Název výpočtu     |  <li>Název je povinný a musí mít délku 3 až 24 znaků.</li><li>Platné znaky jsou velká písmena a malá písmena, číslice a **-** znak.</li><li>Název musí začínat písmenem.</li><li>Název musí být jedinečný v rámci všech stávajících výpočtů v oblasti Azure. Pokud zvolený název není jedinečný, zobrazí se upozornění.</li><li>Pokud **-** se používá znak, musí následovat aspoň jedno písmeno později v názvu.</li>     |
|Typ virtuálního počítače |  Vyberte možnost procesor nebo GPU. Tento typ nelze po vytvoření změnit.     |
|Velikost virtuálního počítače     |  Podporované velikosti virtuálních počítačů můžou být ve vaší oblasti omezené. Kontrolovat [seznam dostupnosti](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines)     |
|Povolit/zakázat přístup přes SSH     |   Přístup SSH je ve výchozím nastavení zakázán.  Přístup SSH nemůže být. Po vytvoření se změnila. Pokud chcete interaktivně ladit pomocí [vs Code vzdálených](how-to-set-up-vs-code-remote.md) , Nezapomeňte povolit přístup.   |
|Pokročilá nastavení     |  Nepovinný parametr. Nakonfigurujte virtuální síť. Zadejte **skupinu prostředků**, **virtuální síť**a **podsíť** pro vytvoření výpočetní instance v rámci Azure Virtual Network (VNET). Další informace najdete v tématu tyto [požadavky na síť](how-to-enable-virtual-network.md#compute-instance) pro virtuální síť.        |

Můžete také vytvořit instanci.
* Přímo z [prostředí integrovaných poznámkových bloků](tutorial-1st-experiment-sdk-setup.md#azure)
* V Azure Portal
* Z šablony Azure Resource Manager. Příklad šablony najdete v tématu [vytvoření Azure Machine Learning šablony výpočetních instancí](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance).
* S [Azure Machine Learning SDK](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-computeinstance/train-on-computeinstance.ipynb)
* Z [rozšíření CLI pro Azure Machine Learning](reference-azure-machine-learning-cli.md#computeinstance)

Vyhrazená jádra na jednu oblast a kvótu pro skupinu virtuálních počítačů a celkovou kvótu, která se vztahuje na vytvoření instance Compute. je sjednocený a sdílený s Azure Machine Learning školením kvóty výpočetních clusterů. Zastavení výpočetní instance neuvolní kvótu, aby bylo zajištěno, že budete moci restartovat výpočetní instanci.

## <a name="compute-target"></a>Cílový výpočetní objekt

Výpočetní instance se dají použít jako [školicí cíl](concept-compute-target.md#train) Azure Machine Learning pro výpočetní prostředky, podobně jako clustery výpočetního školení. 

Výpočetní instance:
* Má frontu úloh.
* Spustí úlohy bezpečně ve virtuálním síťovém prostředí, aniž by museli podniky otevřít port SSH. Úloha se spustí v kontejnerovém prostředí a zabalí závislosti vašich modelů v kontejneru Docker.
* Může spustit více malých úloh paralelně (ve verzi Preview).  Dvě úlohy na jádro můžou běžet paralelně, zatímco zbývající úlohy jsou zařazené do fronty.

Výpočetní instanci můžete použít jako cíl nasazení místní Inferencing pro scénáře testování a ladění.

> [!NOTE]
> Distribuované školicí úlohy nejsou u výpočetní instance podporovány.  Pro distribuované školení použijte (výpočetní clustery) (postupy-nastavení-procvičení-cílení. MD # amlcompute).

Další podrobnosti najdete v poznámkovém bloku s [výukou computeinstance](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-computeinstance/train-on-computeinstance.ipynb). Tento Poznámkový blok je také k dispozici ve složce **ukázek** studia v části *školení/výuka-on-computeinstance*.

## <a name="what-happened-to-notebook-vm"></a><a name="notebookvm"></a>Co se stalo s virtuálním počítačem poznámkového bloku?

Výpočetní instance nahrazují virtuální počítač poznámkového bloku.  

Všechny soubory poznámkových bloků uložené ve sdílené složce pracovního prostoru a data v úložištích dat pracovního prostoru budou přístupná z instance Compute. Všechny vlastní balíčky, které byly dřív nainstalované na virtuálním počítači s poznámkovým blokem, se ale musí na instanci COMPUTE znovu nainstalovat. Omezení kvót, která se vztahují na vytváření výpočetních clusterů, se budou vztahovat i na výpočetní instance.

Nelze vytvořit nové virtuální počítače poznámkového bloku. Máte ale pořád přístup k vytvořeným virtuálním počítačům s poznámkovým blokem s plnou funkčností. Výpočetní instance se dají vytvořit ve stejném pracovním prostoru jako stávající virtuální počítače s poznámkovým blokem.


## <a name="next-steps"></a>Další kroky

 * [Kurz: analýza prvního modelu ml](tutorial-1st-experiment-sdk-train.md) ukazuje, jak používat výpočetní instanci s integrovaným poznámkovým blokem.
