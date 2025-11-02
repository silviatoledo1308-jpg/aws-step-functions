#  Desafio AWS Step Functions - DIO

Este repositório foi desenvolvido como parte do **Desafio AWS Step Functions** da **Digital Innovation One (DIO)**, com o objetivo de consolidar o aprendizado sobre **workflows automatizados na AWS**.

---

##  Tópicos Abordados

Durante o laboratório, foram estudados e aplicados os seguintes conceitos:

* **Conhecendo o AWS Step Functions:**
  Entendimento sobre o serviço, suas finalidades e como ele permite criar fluxos automatizados que integram múltiplos serviços AWS.

* **Benefícios e Projeto Modelo no AWS Step Functions:**
  Exploração das vantagens da orquestração sem servidor, como escalabilidade, redução de custos e facilidade de monitoramento.
  Criação de um projeto modelo para visualizar o funcionamento prático das máquinas de estado.

* **Realizando Validações no AWS Step Functions:**
  Implementação de verificações dentro dos fluxos, utilizando condições lógicas e transições baseadas em resultados, garantindo a execução correta de cada etapa.

* **Criando e Executando Lambda no AWS Step Functions:**
  Integração entre Step Functions e **AWS Lambda**, demonstrando como funções podem ser acionadas automaticamente dentro do fluxo para realizar tarefas específicas.

---

##  Projeto Modelo: Fluxo de Validação e Processamento de Cadastro

O projeto desenvolvido como exemplo consiste em um **workflow automatizado** para validar e processar cadastros de usuários de forma serverless, integrando **AWS Step Functions** com **Lambda**.

###  Etapas do fluxo:

1. **Receber Dados do Cadastro:**
   O processo inicia com a entrada de dados de um novo usuário (nome, e-mail, etc.).

2. **Validação de Dados:**
   Uma função **Lambda** é responsável por validar as informações recebidas (verifica campos obrigatórios, formato de e-mail, etc.).
   Caso a validação falhe, o fluxo é encerrado com erro.

3. **Processamento de Cadastro:**
   Se os dados forem válidos, o fluxo avança para outra função **Lambda** que processa e grava as informações em um banco de dados (por exemplo, DynamoDB).

4. **Confirmação e Encerramento:**
   O processo finaliza exibindo o status **"Sucesso"**, indicando que o cadastro foi processado corretamente.

---

##  Exemplo de Fluxo (State Machine JSON)

```json
{
  "Comment": "Fluxo AWS Step Functions - Validação e Processamento de Cadastro",
  "StartAt": "ValidarDados",
  "States": {
    "ValidarDados": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGIAO:ID_DA_CONTA:function:ValidarCadastro",
      "Next": "VerificarResultado"
    },
    "VerificarResultado": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.valido",
          "BooleanEquals": true,
          "Next": "ProcessarCadastro"
        }
      ],
      "Default": "CadastroInvalido"
    },
    "ProcessarCadastro": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGIAO:ID_DA_CONTA:function:ProcessarCadastro",
      "Next": "Sucesso"
    },
    "CadastroInvalido": {
      "Type": "Fail",
      "Error": "ErroDeValidacao",
      "Cause": "Os dados de entrada são inválidos."
    },
    "Sucesso": {
      "Type": "Succeed"
    }
  }
}
```

**Descrição Técnica:**

* A máquina de estados começa com a função **ValidarDados**.
* Em seguida, verifica se a variável `$.valido` é verdadeira.
* Se for, o fluxo avança para **ProcessarCadastro**.
* Caso contrário, é encerrado com o estado de falha **CadastroInvalido**.
* Quando tudo ocorre corretamente, o fluxo finaliza em **Sucesso**.

---

##  Benefícios do AWS Step Functions

* Integração nativa com diversos serviços AWS;
* Escalabilidade automática e cobrança sob demanda;
* Orquestração visual simples e intuitiva;
* Monitoramento detalhado de execuções;
* Ideal para automação de processos e microserviços.

---

##  Aprendizados e Conclusões

Esse desafio permitiu:

* Consolidar o entendimento sobre o funcionamento das **Step Functions**;
* Aprender a criar fluxos automatizados de forma prática;
* Entender como o **AWS Lambda** pode ser integrado em processos serverless;
* Praticar a criação de **condições e validações dentro do fluxo**;
* Fortalecer a habilidade de **documentar e versionar projetos técnicos** no GitHub.


---

##  Referências

* [Documentação Oficial AWS Step Functions](https://docs.aws.amazon.com/step-functions/)
* [Amazon States Language](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-amazon-states-language.html)
* [AWS Lambda - Documentação](https://docs.aws.amazon.com/lambda/)
* [AWS IAM - Funções de Execução](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)

---

**Autor:** Silvia Toledo
