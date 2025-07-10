# Challenge-Telecom-X-an-lise-de-evas-o-de-clientes
📊 Análise de Churn de Clientes TelecomX
Este projeto tem como objetivo principal conduzir uma análise aprofundada sobre a evasão de clientes (churn) da empresa Telecom X. Através da identificação de padrões de comportamento e características dos clientes que demonstram maior propensão ao cancelamento de serviços, busca-se subsidiar a empresa na formulação de estratégias de retenção eficazes.

🛠️ Ferramentas e Tecnologias
O desenvolvimento deste projeto foi realizado utilizando as seguintes ferramentas e bibliotecas:

Python: Linguagem de programação fundamental para a execução das análises.

Pandas: Biblioteca essencial para manipulação e análise de dados tabulares.

Seaborn e Matplotlib: Bibliotecas empregadas para a criação de visualizações de dados informativas e esteticamente relevantes.

🚀 Metodologia
A metodologia aplicada neste projeto compreende as seguintes etapas:

📌 1. Aquisição de Dados
Os dados foram extraídos de um arquivo JSON hospedado em um repositório GitHub, sendo carregados para um DataFrame Pandas através da função pd.read_json().

import pandas as pd

url = 'https://raw.githubusercontent.com/alura-cursos/challenge2-data-science/main/TelecomX_Data.json'
df = pd.read_json(url)
# Verificação inicial dos dados
# df.head()
# df.info()
# df.isnull().sum()
# df.dtypes

🔧 2. Processamento e Transformação de Dados
Esta fase é crucial para a preparação dos dados brutos, visando sua adequação para a análise subsequente. As principais operações de transformação incluíram:

Normalização de Estruturas Aninhadas: As colunas customer, phone, internet, e account, que continham estruturas de dados aninhadas (dicionários), foram normalizadas utilizando pd.json_normalize(), resultando em colunas planas no DataFrame principal.

Remoção de Colunas Originais: As colunas aninhadas originais foram descartadas após a sua normalização.

Conversão de Tipos de Dados: Colunas como 'Charges.Monthly', 'Charges.Total' e 'tenure' tiveram seus tipos de dados convertidos para numérico, com tratamento de erros para valores não-numéricos.

# Normalização das colunas aninhadas
customer_df = pd.json_normalize(df['customer'])
df = pd.concat([df, customer_df], axis=1)

phone_df = pd.json_normalize(df['phone'])
df = pd.concat([df, phone_df], axis=1)

internet_df = pd.json_normalize(df['internet'])
df = pd.concat([df, internet_df], axis=1)

account_df = pd.json_normalize(df['account'])
df = pd.concat([df, account_df], axis=1)

# Remoção das colunas aninhadas originais
df.drop(columns=['customer', 'phone', 'internet', 'account'], inplace=True)

# Conversão de tipos de dados para numérico
df['Charges.Monthly'] = pd.to_numeric(df['Charges.Monthly'], errors='coerce')
df['Charges.Total'] = pd.to_numeric(df['Charges.Total'], errors='coerce')
df['tenure'] = pd.to_numeric(df['tenure'], errors='coerce')

📊 3. Análise Exploratória e Visualização
Com os dados devidamente preparados, foram geradas visualizações para compreender a distribuição do churn e sua correlação com variáveis relevantes.

Distribuição de Churn: Um gráfico de contagem foi utilizado para ilustrar a proporção de clientes que cancelaram o serviço em relação aos que permaneceram.

Churn por Faixa Etária: Um histograma empilhado foi empregado para analisar a influência da idade na decisão de churn.

import seaborn as sns
import matplotlib.pyplot as plt

# Distribuição de Churn
sns.countplot(x='Churn', data=df)
plt.title('Distribuição de Churn')
plt.show()

# Churn por Faixa Etária (SeniorCitizen)
sns.histplot(data=df, x='SeniorCitizen', hue='Churn', multiple='stack')
plt.title('Churn por Faixa Etária')
plt.show()

✅ Sumário Executivo: Principais Descobertas
O presente relatório de análise revelou os seguintes pontos críticos:

Contratos Mensais: Clientes com contratos de modalidade mensal demonstram uma maior propensão ao churn, indicando uma correlação entre contratos de curto prazo e a taxa de evasão.

Clientes Seniores: Indivíduos com 65 anos ou mais (SeniorCitizen) apresentam uma taxa de churn elevada, sugerindo a necessidade de atenção direcionada a este segmento demográfico.

💡 Recomendações e Próximos Passos
Com base nas análises realizadas, as seguintes recomendações são propostas para a Telecom X:

Implementar programas de incentivo para contratos de longo prazo, visando a fidelização dos clientes.

Desenvolver um programa de suporte ou ofertas personalizadas para o segmento de clientes seniores, a fim de mitigar a taxa de churn nesse grupo.

Estabelecer um monitoramento contínuo dos indicadores de churn, permitindo a identificação precoce de novas tendências e a implementação ágil de ações preventivas.

Este projeto foi desenvolvido como parte de um desafio de análise de dados para a Telecom X.
