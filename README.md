# Exportação de Dados de Billing da Google Cloud Platform (GCP)

Este documento detalha o processo passo a passo para exportar dados de billing da sua conta Google Cloud Platform (GCP) para o BigQuery ou Cloud Storage. A exportação de dados de billing permite analisar seus custos em detalhes, criar relatórios personalizados e integrar com outras ferramentas de análise de dados.

## Pré-requisitos

*   Você precisa ter uma conta de billing ativa na GCP.
*   Você precisa ter permissões para configurar a exportação de dados de billing. Geralmente, o papel `billing.resourceUsageReports.create` e `billing.resourceUsageReports.get` são suficientes.
*   Para exportar para o BigQuery:
    *   Você precisa de um projeto GCP com o BigQuery API habilitado.
    *   Você precisa ter permissões para criar tabelas no BigQuery.
*   Para exportar para o Cloud Storage:
    *   Você precisa de um bucket do Cloud Storage existente.
    *   Você precisa ter permissões para escrever no bucket do Cloud Storage.

## Escolhendo o Destino da Exportação

Você tem duas opções principais para exportar seus dados de billing:

1.  **BigQuery:** Ideal para análise de dados complexas, consultas SQL e criação de painéis.
2.  **Cloud Storage:** Ideal para armazenamento de dados de longo prazo, arquivamento e integração com outras ferramentas que consomem arquivos CSV ou JSON.

Escolha o destino que melhor se adapta às suas necessidades.

## Passo 1: Acessando a página de Billing

1.  **Acesse o Console do Google Cloud:** Abra seu navegador e vá para [https://console.cloud.google.com/](https://console.cloud.google.com/).
2.  **Selecione sua organização ou projeto:** Certifique-se de ter selecionado a organização ou projeto correto no seletor na parte superior da tela.
3.  **Navegue até Billing:** No menu de navegação à esquerda, clique em "Billing". Se a opção "Billing" não estiver visível, clique em "Mais Produtos" e procure por ela.

## Passo 2: Habilitando a Exportação de Dados de Billing

1.  **Selecione a Conta de Billing:** Na página de Billing, selecione a conta de billing para a qual você deseja configurar a exportação de dados.
2.  **Acesse a seção de Exportação de Dados de Billing:** No menu de navegação à esquerda da página de Billing, clique em "Exportação de Dados de Billing".
3.  **Selecione o Tipo de Exportação:** Você verá duas abas: "Uso detalhado" e "Custos padrão".  A aba "Uso detalhado" fornece informações mais granulares sobre o uso dos recursos, enquanto a aba "Custos padrão" oferece uma visão geral dos seus gastos.  Escolha a aba que melhor se adapta às suas necessidades. Geralmente, a aba "Uso detalhado" é preferível para análises mais aprofundadas.

## Passo 3: Configurando a Exportação para o BigQuery

1.  **Habilite a Exportação para BigQuery:** Na aba selecionada (Uso detalhado ou Custos padrão), clique em "EDITAR CONFIGURAÇÃO" sob o título "BigQuery export".
2.  **Selecione o Projeto do BigQuery:** Escolha o projeto GCP que contém o BigQuery dataset onde você deseja armazenar os dados de billing.
3.  **Selecione o Dataset do BigQuery:** Selecione um dataset existente ou crie um novo.
    *   **Criar um novo dataset:** Se você precisa criar um novo dataset, clique em "Criar um conjunto de dados". Forneça um nome para o dataset, uma localização e uma política de expiração (opcional).
4.  **Nome da Tabela (opcional):** Você pode especificar um prefixo para o nome da tabela. Se você não especificar um prefixo, o GCP criará tabelas com nomes padrão.
5.  **Incluir dados de custos aplicados (opcional):** Marque essa opção se você deseja incluir detalhes sobre os descontos e créditos aplicados à sua conta.
6.  **Clique em "SALVAR":** O GCP começará a exportar os dados de billing para o BigQuery. A primeira exportação pode levar algum tempo, dependendo da quantidade de dados.

## Passo 4: Configurando a Exportação para o Cloud Storage

1.  **Habilite a Exportação para Cloud Storage:** Na aba selecionada (Uso detalhado ou Custos padrão), clique em "EDITAR CONFIGURAÇÃO" sob o título "Cloud Storage export".
2.  **Selecione o Bucket do Cloud Storage:** Escolha o bucket do Cloud Storage onde você deseja armazenar os arquivos de billing.
3.  **Prefixo do Caminho do Arquivo (opcional):** Você pode especificar um prefixo para o nome dos arquivos. Isso ajuda a organizar seus dados no bucket.
4.  **Formato do Arquivo:** Selecione o formato do arquivo: CSV ou JSON.
5.  **Incluir dados de custos aplicados (opcional):** Marque essa opção se você deseja incluir detalhes sobre os descontos e créditos aplicados à sua conta.
6.  **Clique em "SALVAR":** O GCP começará a exportar os dados de billing para o Cloud Storage. A primeira exportação pode levar algum tempo, dependendo da quantidade de dados.

## Passo 5: Verificando a Exportação

**Para BigQuery:**

1.  Acesse o console do BigQuery no projeto que você selecionou.
2.  Verifique se a tabela (ou tabelas) de billing foi criada no dataset que você especificou.
3.  Execute uma consulta simples para verificar se os dados estão sendo exportados corretamente.

**Para Cloud Storage:**

1.  Acesse o console do Cloud Storage.
2.  Navegue até o bucket que você selecionou.
3.  Verifique se os arquivos de billing estão sendo criados no bucket.

## Exemplo de Consulta BigQuery

Aqui está um exemplo de consulta BigQuery para obter os custos totais por projeto:

```sql
SELECT
  project.name,
  SUM(cost) AS total_cost
FROM
  `seu-projeto.seu_dataset.gcp_billing_export_v1_*`
GROUP BY
  project.name
ORDER BY
  total_cost DESC
```

**Substitua:**

*   `seu-projeto` pelo ID do seu projeto GCP.
*   `seu_dataset` pelo nome do seu dataset BigQuery.
*   `gcp_billing_export_v1_*` pelo nome da sua tabela de billing (ou use um curinga para consultar todas as tabelas).  O `_v1` significa que você está usando a versão 1 do esquema da tabela. Se você usa a versão 2, substitua por `_v2`.

## Dicas e Boas Práticas

*   **Use o esquema da tabela:** Familiarize-se com o esquema da tabela de billing para entender os diferentes campos e como eles se relacionam.  A documentação da GCP fornece detalhes sobre o esquema.
*   **Monitore seus gastos regularmente:** Use os dados exportados para criar relatórios personalizados e painéis para monitorar seus gastos na GCP.
*   **Automatize a análise de dados:** Use ferramentas como Cloud Functions e Cloud Scheduler para automatizar a análise de dados de billing e gerar relatórios automaticamente.
*   **Versionamento de dados:** Considere usar o versionamento de objetos no Cloud Storage para manter um histórico dos seus dados de billing.
*   **Segurança:** Proteja seus dados de billing armazenados no BigQuery e Cloud Storage com as permissões apropriadas.
*   **Custos da Exportação:** Esteja ciente de que a exportação de dados para o BigQuery e Cloud Storage pode gerar custos.  O custo dependerá da quantidade de dados que você exporta e dos preços do BigQuery e Cloud Storage.

## Solução de Problemas

*   **Os dados não estão sendo exportados:**
    *   Verifique se você habilitou a exportação de dados de billing na página de Billing.
    *   Verifique se as permissões estão configuradas corretamente para o projeto do BigQuery ou o bucket do Cloud Storage.
    *   Verifique se o BigQuery API está habilitado no projeto do BigQuery.
    *   Verifique os logs do Cloud Logging para erros relacionados à exportação de dados de billing.
*   **A consulta BigQuery está falhando:**
    *   Verifique se você está usando o nome correto da tabela no BigQuery.
    *   Verifique se o esquema da tabela é o esperado.
    *   Verifique se você tem as permissões necessárias para consultar a tabela.
*   **Os arquivos no Cloud Storage estão faltando dados:**
    *   Verifique se você selecionou o formato de arquivo correto (CSV ou JSON).
    *   Verifique se você habilitou a opção "Incluir dados de custos aplicados", se necessário.

Seguindo este guia, você poderá configurar a exportação de dados de billing da GCP e obter informações valiosas sobre seus custos na nuvem. Lembre-se de revisar a documentação oficial da GCP para obter informações mais detalhadas e atualizadas.
```
**Observações:**

*   **Documentação Oficial:** Sempre consulte a documentação oficial da GCP para obter as informações mais recentes e precisas.  Os links estão incluídos no README.
*   **Custos:** É importante estar ciente dos custos associados ao BigQuery e ao Cloud Storage.  O custo dependerá da quantidade de dados armazenados e processados.
*   **Segurança:**  Implemente medidas de segurança adequadas para proteger seus dados de billing.

Este README fornece um guia completo para a exportação de dados de billing da GCP.

Adapte-o às suas necessidades específicas e consulte a documentação da GCP para obter informações mais detalhadas.
