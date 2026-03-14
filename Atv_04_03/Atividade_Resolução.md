# PARTE 04 — Casos de Teste

## Caso de Teste 1

**ID:** CT-001  
**Título/Objetivo:** Depósito com valor 0  

**Pré-condição:**  
Possuir a conta "tony" criada e existente no sistema.

**Passos:**
1. Executar: `banco.depositar('tony', 0)`
2. Consultar saldo: `banco.saldo_atual('tony')`

**Resultado Esperado:**  
O sistema deve rejeitar o depósito e exibir uma mensagem de erro informando que o valor deve ser maior que zero. O saldo não deve ser alterado.

**Resultado Obtido:**  
O sistema exibiu a mensagem **"OK - Depósito realizado: 0"** e o saldo foi alterado posteriormente após o depósito nulo.

**Status:** Fail  

**Evidência:**
```
Teste N1 - Depósito com valor zero
OK - Depósito realizado: 0
```

---

## Caso de Teste 2

**ID:** CT-002  
**Título/Objetivo:** Depósito negativo  

**Pré-condição:**  
Possuir a conta "tony" criada e existente no sistema.

**Passos:**
1. Executar: `banco.depositar('tony', -100)`
2. Consultar saldo: `banco.saldo_atual('tony')`

**Resultado Esperado:**  
O sistema deve rejeitar o depósito e exibir uma mensagem de erro informando que o valor deve ser maior que zero. O saldo não deve ser alterado.

**Resultado Obtido:**  
O sistema exibiu a mensagem **"OK - Depósito realizado: -100"** e o saldo foi reduzido para **600.0**.

**Status:** Fail  

**Evidência:**
```
Teste N2 - Depósito negativo
OK - Depósito realizado: -100

Saldo após depósitos inválidos
SALDO tony = 600.0
```

---

## Caso de Teste 3

**ID:** CT-003  
**Título/Objetivo:** Saque acima do saldo  

**Pré-condição:**  
Possuir a conta "tony" criada e existente no sistema.

**Passos:**
1. Executar: `banco.sacar('tony', 100000)`

**Resultado Esperado:**  
O sistema deve impedir o saque, exibir mensagem de saldo insuficiente e manter o saldo inalterado.

**Resultado Obtido:**  
O sistema exibiu a mensagem **"ERRO - Saldo insuficiente"** e o saldo permaneceu inalterado.

**Status:** Pass  

**Evidência:**
```
Teste N3 - Saque acima do saldo
ERRO - Saldo insuficiente
```

---

# PARTE 05 — Bug Report

## Bug 1

**Título:** Depósito com valor 0  

**Severidade:** Alta  
**Prioridade:** Alta  
**Ambiente:** Google Colab / Python 3.x / Banco do Stark  

**Descrição:**  
O sistema aceita depósito com valor igual a zero, contrariando a regra de negócio que define que o valor deve ser maior que zero.

**Passos para Reproduzir:**
1. Executar: `banco.depositar('tony', 0)`
2. Consultar saldo: `banco.saldo_atual('tony')`

**Resultado Esperado:**  
O sistema deve rejeitar o depósito e exibir mensagem de erro. O saldo não deve ser alterado.

**Resultado Obtido:**  
O sistema exibiu **"OK - Depósito realizado: 0"** e permitiu a operação.

**Evidência:**
```
Teste N1 - Depósito com valor zero
OK - Depósito realizado: 0
```

**Impacto/Risco:**  
Pode gerar inconsistências contábeis e permitir registros inválidos no extrato.

---

## Bug 2

**Título:** Depósito com valor negativo  

**Severidade:** Alta  
**Prioridade:** Alta  
**Ambiente:** Google Colab / Python 3.x / Banco do Stark  

**Descrição:**  
O sistema aceita depósito com valor negativo, reduzindo o saldo da conta indevidamente.

**Passos para Reproduzir:**
1. Executar: `banco.depositar('tony', -100)`
2. Consultar saldo: `banco.saldo_atual('tony')`

**Resultado Esperado:**  
O sistema deve rejeitar o depósito negativo e exibir mensagem de erro.

**Resultado Obtido:**  
O sistema exibiu **"OK - Depósito realizado: -100"** e reduziu o saldo para 600.0.

**Evidência:**
```
Teste N2 - Depósito negativo
OK - Depósito realizado: -100
SALDO tony = 600.0
```

**Impacto/Risco:**  
Permite manipulação indevida de saldo e compromete a integridade financeira do sistema.

---

## Bug 3

**Título:** Ausência de validação para valor mínimo em depósito  

**Severidade:** Média  
**Prioridade:** Média  
**Ambiente:** Google Colab / Python 3.x / Banco do Stark  

**Descrição:**  
O método de depósito não valida se o valor informado é maior que zero antes de realizar a operação, permitindo valores inválidos.

**Passos para Reproduzir:**
1. Executar: `banco.depositar('tony', 0)`
2. Executar: `banco.depositar('tony', -100)`

**Resultado Esperado:**  
O sistema deve validar o valor antes de alterar o saldo e exibir mensagem de erro quando o valor for menor ou igual a zero.

**Resultado Obtido:**  
O sistema executa a operação normalmente, exibindo mensagem de sucesso mesmo com valores inválidos.

**Evidência:**
```
Teste N1 - Depósito com valor zero
OK - Depósito realizado: 0

Teste N2 - Depósito negativo
OK - Depósito realizado: -100
```

**Impacto/Risco:**  
Descumprimento da regra de negócio e risco de inconsistência financeira no sistema.

---
