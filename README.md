# prova2sistemasdistribuidos

## 1. Situação-problema
Descreva brevemente os problemas apresentados na Central de Pedidos Distribuidos


Ao todo foram 3

1 - Era necessario que apenas 3 pedidos fossem processado

2 - A replica não era atualizado junto ao principal

3 - Sempre era mostrado erro até quando está correto.


## 2. Primeira execução

Explique o que foi observado ao executar o programa pela primeira vez.

O programa estava todo quebrado, 
Basicamente o servidor de replica nãpo era atualizado, eram processados mais de dois pedidos ao mesmo tempo e Não era mostrado o status do servidor de replica e não tinha tratativa de erro.

## Evidencias

### Evidencia 1 - Primeira execução

Está como arquivo de texto,

Arquivo completamente incorreto

================CENTRAL DE PEDIDOS================
limite simultaneo: 3
Estoque Inicial: 10
Pedidos confirmados: [101, 102, 103]
Estoque principal: 7
Replica antes: 10
Replica depois: 10
Consulta apos falha: INDISPONIVEL None

## 3. Problema A - Concorrência

Qual problema doi identificado?

MAXIMO_SIMULTANEO = 3 

Trocado para

MAXIMO_SIMULTANEO = 2


### Evidencia 2 - Concorrência

Está como arquivo de texto

================CENTRAL DE PEDIDOS================
limite simultaneo: 2
Estoque Inicial: 10
Pedidos confirmados: [101, 102, 103]
Estoque principal: 7
Replica antes: 10
Replica depois: 10
Consulta apos falha: INDISPONIVEL None

## 4. Problema B - Replicação e consistência

Qual problema foi identificado?

principal["estoque"] = principal["estoque"]

Trocado para

replica["estoque"] = principal["estoque"]


### Evidencia 3 - Sincronização

Está como arquivo de texto

================CENTRAL DE PEDIDOS================
limite simultaneo: 2
Estoque Inicial: 10
Pedidos confirmados: [101, 102, 103]
Estoque principal: 7
Replica antes: 10
Replica depois: 7
Consulta apos falha: INDISPONIVEL None

## 5. Problema C - Tolerância a Falhas

Qual problema foi identificado?

if principal["online"]:
            return(
                "REPLICA",
                replica["estoque"]
            )

Trocado para

if replica["online"]:
            return(
                "REPLICA",
                replica["estoque"]
            )

### Evidencia 4 - Tolerância a falhas

Está como arquivo de texto

================CENTRAL DE PEDIDOS================
limite simultaneo: 2
Estoque Inicial: 10
Pedidos confirmados: [101, 102, 103]
Estoque principal: 7
Replica antes: 10
Replica depois: 7
Consulta apos falha: REPLICA 7

### Evidencia 5 - Execução completa

================CENTRAL DE PEDIDOS================
limite simultaneo: 2
Estoque Inicial: 10
Pedidos confirmados: [101, 102, 103]
Estoque principal: 7
Replica antes: 10
Replica depois: 7
Consulta apos falha: REPLICA 7


## 6. Validação do Programa

após as três modificações o programa rodou conforme a solicitação do Cliente

## 7. Análise Técnica

### Questão 1
Qual é a Função do semáforo no processamento dos pedidos?

Permitir que os theads sejam cordenados ao tentarem fazer a requisição, seria quase que colocar eles em uma fila indiana

### Questão 2
Por que é necessário sincronizar os dados entre o servidor principal e a réplica?

Se o servidor principal cair ou falhar a réplica ira atuar como Backup para o processamento

### Questão 3
O que significa tolerância a falhas e como ela aparece neste programa?

Tolerancia a falhas serve para conseguirmos apontar para o servidor2 para qualquer estabilidade do primeiro servidor

## Questão 4
Para que serve o lock no programa e como ele ajuda a prevenir problemas de concorreência?

Basicamente, o lock não permite que os theads façam a requisão simultaneamente, ou seja, apenas um de cada vez fará o seu processamento

### Questão 5
A sincronização do estoque deve acontecer antes ou depois do processamento dos pedidos?
Explique a relação de casualidade

Obviamente após, assim que o processamento dos pedidos é feito deve-se replicar o processamento do servidor principal no segundo, se acontecer antes desse processo a replica não é atualizada

## 8. Conclusão

Descreva o que você aprendeu com esta atividade.

É de extrema importancia que os theads sejam, enfileirado e permitidos apenas um por vez, Além disso a replicação dos dados no servidor é uma questão de segura (muito importante em um ambiente de PRD), bem como que o tratamento a falhas é tão importante quanto replicar os dados, afinal só assim em caso de falha ele será utilizado.

## 9. Entrega

**Aluno: Ryan Cirino da Silva
** Repositorio:** https://github.com/ryancirinodasilva/prova2sistemasdistribuidos
**Data:** 18/112026
