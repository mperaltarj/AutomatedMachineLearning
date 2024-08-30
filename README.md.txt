# Tutorial: Criando e Utilizando um Workspace no Azure Machine Learning

Este tutorial guia você na criação de um workspace do Azure Machine Learning, no treinamento de um modelo de machine learning automatizado, e na implantação do modelo para previsões em tempo real. 

## 1. Criando um Workspace no Azure Machine Learning

### Passo 1: Acessar o Portal do Azure

- Acesse [https://portal.azure.com](https://portal.azure.com) com suas credenciais Microsoft.

### Passo 2: Criar o Recurso de Machine Learning

1. Selecione **+ Criar um recurso**, pesquise por **Machine Learning**, e crie um novo recurso com as seguintes configurações:
   - **Assinatura**: Selecione sua assinatura do Azure.
   - **Grupo de Recursos**: Crie ou selecione um grupo de recursos.
   - **Nome**: Insira um nome único para seu workspace.
   - **Região**: Selecione a região geográfica mais próxima.
   - **Conta de Armazenamento**: Um novo armazenamento será criado automaticamente.
   - **Key Vault**: Um novo Key Vault será criado automaticamente.
   - **Application Insights**: Será criado automaticamente.
   - **Registro de Container**: Não selecione nada (um será criado automaticamente ao implantar um modelo).

2. Selecione **Revisar + Criar** e, em seguida, **Criar**. Aguarde a criação do workspace.

### Passo 3: Acessar o Azure Machine Learning Studio

- Após a criação do workspace, clique em **Launch studio** ou acesse [https://ml.azure.com](https://ml.azure.com) e entre com sua conta Microsoft.

## 2. Usando AutoML para Treinar um Modelo

### Passo 1: Criar um Job de AutoML

1. No Azure Machine Learning Studio, acesse **Automated ML** na seção de **Authoring**.
2. Crie um novo job de AutoML com as seguintes configurações:

   **Configurações Básicas**:
   - **Nome do Job**: `mslearn-bike-automl`
   - **Nome do Experimento**: `mslearn-bike-rental`
   - **Descrição**: `Automated machine learning for bike rental prediction`

   **Tipo de Tarefa & Dados**:
   - **Tipo de Tarefa**: Regressão
   - **Dataset**: Crie um novo dataset:
     - **Nome**: `bike-rentals`
     - **Descrição**: `Historic bike rental data`
     - **Tipo de Dado**: Tabela (mltable)
     - **Fonte de Dados**: Arquivos Locais
     - **Tipo de Armazenamento**: Azure Blob Storage
     - **Nome do Datastore**: `workspaceblobstore`
     - **Upload Folder**: Baixe e descompacte [estes arquivos](https://aka.ms/bike-rentals).

   **Configurações da Tarefa**:
   - **Coluna Alvo**: `rentals`
   - **Métrica Primária**: `NormalizedRootMeanSquaredError`
   - **Modelos Permitidos**: Selecione apenas **RandomForest** e **LightGBM**.
   - **Limites**:
     - **Máximo de Testes**: 3
     - **Máximo de Testes Simultâneos**: 3
     - **Timeout do Experimento**: 15 minutos
     - **Timeout da Iteração**: 15 minutos
   - **Validação**: Split de treinamento e validação (10% de dados de validação).

   **Computação**:
   - **Tipo de Computação**: Serverless
   - **Tipo de VM**: CPU
   - **Tamanho da VM**: `Standard_DS3_v2`
   - **Número de Instâncias**: 1

3. Submeta o job e aguarde sua conclusão.

## 3. Revisar o Melhor Modelo

- Ao finalizar o job, revise o modelo no tab **Overview** do job de AutoML.
- Verifique os gráficos de **residuals** e **predicted_true** na aba **Metrics**.

## 4. Implantar e Testar o Modelo

### Passo 1: Implantar o Modelo

1. No tab **Model** do melhor modelo, selecione **Deploy** e utilize as seguintes configurações:
   - **Nome da Endpoint**: Deixe o padrão ou escolha um nome único.
   - **Inferência**: Desativado
   - **Empacotar Modelo**: Desativado

2. Aguarde a implantação ser concluída.

### Passo 2: Testar o Serviço

1. No Azure Machine Learning Studio, acesse **Endpoints** e selecione a endpoint **predict-rentals**.
2. Na aba **Test**, substitua o JSON template pelo seguinte:

```json
{
  "input_data": {
    "columns": [
      "day", "mnth", "year", "season", "holiday", "weekday", 
      "workingday", "weathersit", "temp", "atemp", "hum", "windspeed"
    ],
    "index": [0],
    "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
  }
}

Clique em Test e revise o resultado predito.
5. Limpeza
Para evitar custos, delete a endpoint predict-rentals na aba Endpoints.
Para deletar o workspace, acesse o grupo de recursos no Portal do Azure, clique em Excluir Grupo de Recursos, e confirme.

Este tutorial foi criado para guiar na configuração e uso básico do Azure Machine Learning. Para mais detalhes e funcionalidades avançadas, consulte a documentação oficial.


Este `README.md` fornece instruções claras e detalhadas em português para configurar um workspace de Azure Machine Learning, treinar um modelo usando AutoML, implantar e testar o modelo, e limpar os recursos para evitar cobranças desnecessárias.

Fonte: https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html