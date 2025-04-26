# Projeto: Coleta de Dados de Criptomoedas 🚀

Este projeto coleta informações de preços e características de criptomoedas a partir da API pública da CoinGecko e salva os dados no **BigQuery** ou em arquivos **CSV**, conforme configuração.

## Premissas
1. O Crypto Market Cap está deprecado.

  ![image](https://github.com/user-attachments/assets/e5060730-ed3d-4d9e-96c8-6ba86a38d666)

2. Por isso foi usada outra API, a CoinGecko (Link do endpoint: (https://api.coingecko.com/api/v3/coins/markets))
  
3. O script que eu enviei é baseado na documentação oficial do BigQuery para carregar dados de um DataFrame para uma tabela no BigQuery. Ele foi adaptado para case, logo não é uma cópia exata dos exemplos da documentação do BigQuery. Porém, abaixo estão alguns itens do script que são comuns e encontrados na documentação oficial:
  - Carregamento de dados para o BigQuery: O BigQuery fornece a função load_table_from_dataframe para carregar dados de um pandas.DataFrame diretamente para uma tabela no BigQuery
    ```bash
      job = client.load_table_from_dataframe(df, table_id, job_config=job_config)
      job.result()

  - Configuração de carga (LoadJobConfig): A configuração job_config define como os dados devem ser carregados. O parâmetro autodetect=True permite que o BigQuery detecte automaticamente os tipos de dados, e o write_disposition="WRITE_APPEND" especifica que os dados devem ser anexados à tabela existente (em vez de sobrescrevê-la).
    ```bash
      job_config = bigquery.LoadJobConfig(
          write_disposition="WRITE_APPEND",
          autodetect=True
      )

- Uso do BigQuery Client: O BigQuery Client (google.cloud.bigquery.Client()) é usado para interagir com o serviço e fazer o upload dos dados.



## 📋 Funcionalidades

- Coleta dados de mercado das principais criptomoedas.
- Prepara os dados adicionando timestamp UTC.
- Armazena os dados em duas tabelas distintas:
  - **currencies** (informações básicas das moedas)
  - **currency_prices** (preços e volumes)
- Opção de salvar localmente em CSV ao invés de enviar para o BigQuery.

## 🛠️ Configuração do Ambiente

1. **Clone o repositório**:
   ```bash
   git clone https://github.com/seu-usuario/seu-repositorio.git
   cd seu-repositorio

2. **Crie e ative um ambiente virtual (opcional, mas recomendado)**:
   ```bash
    python -m venv venv
    source venv/bin/activate  # Mac/Linux
    venv\Scripts\activate     # Windows
   
3. **Instale as dependências**:
    ```bash
      pip install pandas requests google-cloud-bigquery google-cloud-storage

4. **Configurar credenciais do Google Cloud**:
   - Crie um arquivo de Service Account no console do Google Cloud com permissões para acesso ao BigQuery.
   - Atualize a variável key_path no script com o caminho para o seu arquivo .json.
   - Atualize também o project_id e dataset_id no código para seu projeto/dataset.

## ⚙️ Como Executar
1. **Edite as configurações no script**:
   - Defina load_to_bigquery = True para enviar dados para o BigQuery ou False para salvar localmente em CSV.
2. **Execute o script**:
    ```bash
      python crypto.ipynb

**Saída esperada**:

![image](https://github.com/user-attachments/assets/34e0ab1a-96ba-43fa-a059-d8fb47534efa)
![image](https://github.com/user-attachments/assets/2cfa4a99-593b-4b84-9c75-b9c81641bf88)
![image](https://github.com/user-attachments/assets/9351c5db-ecfb-443b-a825-7a4dbdef512c)

**Tabelas criadas no BigQuery**:

![image](https://github.com/user-attachments/assets/7e414b9e-62d2-42f6-9ece-6c27bd7948d4)



## 📈 Dashboard Power BI

![image](https://github.com/user-attachments/assets/e11b0832-57a9-4b6a-a2a4-0792b8765496)


![image](https://github.com/user-attachments/assets/ea8ac3a5-f7d4-4951-903b-00ce1aaecd96)






## 🚨 Observações
- O script coleta no máximo 4 páginas (1000 criptos) para evitar erros de excesso de requisições (HTTP 429).

- Lembrar de configurar corretamente a Service Account para evitar problemas de autenticação.

- Em breve, o BigQuery exigirá a instalação do pacote pandas-gbq e fica enviando logs de warning. Para evitar esses logs de warnings:
  ```bash
    pip install pandas-gbq
