# Atividade em Aula — QA Manual  
## Banco do Stark

---

## Objetivo da Atividade

- Transformar a execução prática em **Casos de Teste**
- Registrar pelo menos **1 Bug Report profissional**

---

## Como Vamos Trabalhar

1. Execute as células na ordem.
2. Observe os resultados dos testes (saídas no notebook).
3. Preencha os templates:
   - Casos de Teste
   - Bug Report

---

## Entrega (durante a aula)

- Mínimo **3 Casos de Teste** preenchidos  
  (com resultado esperado, resultado obtido e status)

- Mínimo **3 Bug Report** preenchido  
  (com passos reproduzíveis e evidência)

---

## Regras Esperadas (Requisitos do Banco)

Considere que o comportamento correto do sistema deve ser:

- O titular não pode ser vazio (nem só espaços).
- Depósito: valor deve ser > 0.
- Saque: valor deve ser > 0 e não pode exceder o saldo.
- Transferência:
  - valor deve ser > 0
  - destino deve existir
  - não pode transferir para si mesmo
- Saldo nunca pode ficar negativo.
- Extrato deve registrar corretamente as operações.

---

# Código da Aplicação

```python
# =========================================
# APP: Banco do Stark (simples)
# =========================================

class Conta:
    def __init__(self, titular: str):
        self.titular = titular
        self.saldo = 0.0
        self.extrato = []


class BancoDoStark:
    def __init__(self):
        self.contas = {}

    def criar_conta(self, titular: str):
        if titular in self.contas:
            print('ERRO - Conta já existe:', titular)
            return
        self.contas[titular] = Conta(titular)
        print('OK - Conta criada para:', titular)

    def depositar(self, titular: str, valor: float):
        if titular not in self.contas:
            print('ERRO - Conta não encontrada:', titular)
            return
        self.contas[titular].saldo += valor
        self.contas[titular].extrato.append(('deposito', valor))
        print('OK - Depósito realizado:', valor)

    def sacar(self, titular: str, valor: float):
        if titular not in self.contas:
            print('ERRO - Conta não encontrada:', titular)
            return
        if self.contas[titular].saldo < valor:
            print('ERRO - Saldo insuficiente')
            return
        self.contas[titular].saldo -= valor
        self.contas[titular].extrato.append(('saque', valor))
        print('OK - Saque realizado:', valor)

    def transferir(self, origem: str, destino: str, valor: float):
        if origem not in self.contas:
            print('ERRO - Conta origem não encontrada')
            return
        if destino not in self.contas:
            print('ERRO - Conta destino não encontrada')
            return
        if self.contas[origem].saldo < valor:
            print('ERRO - Saldo insuficiente')
            return
        self.contas[origem].saldo -= valor
        self.contas[destino].saldo += valor
        self.contas[origem].extrato.append(('transferencia_saida', valor))
        self.contas[destino].extrato.append(('transferencia_entrada', valor))
        print('OK - Transferência realizada:', valor)

    def saldo_atual(self, titular: str):
        if titular not in self.contas:
            print('ERRO - Conta não encontrada:', titular)
            return
        print('SALDO', titular, '=', self.contas[titular].saldo)

    def mostrar_extrato(self, titular: str):
        if titular not in self.contas:
            print('ERRO - Conta não encontrada:', titular)
            return
        print('EXTRATO:', titular)
        for item in self.contas[titular].extrato:
            print(' -', item)


banco = BancoDoStark()
print('Banco do Stark carregado!')
```

---

# Execução Guiada

```python
# =========================================
# CENÁRIO INICIAL (caminho feliz)
# =========================================

print('=== CRIAR CONTAS ===')
banco.criar_conta('tony')
banco.criar_conta('pepper')
banco.criar_conta('banner')

print('\n=== DEPÓSITOS ===')
banco.depositar('tony', 1000)
banco.depositar('pepper', 500)

print('\n=== SALDOS ===')
banco.saldo_atual('tony')
banco.saldo_atual('pepper')

print('\n=== SAQUE ===')
banco.sacar('tony', 200)
banco.saldo_atual('tony')

print('\n=== TRANSFERÊNCIA ===')
banco.transferir('tony', 'pepper', 100)
banco.saldo_atual('tony')
banco.saldo_atual('pepper')

print('\n=== EXTRATO (tony) ===')
banco.mostrar_extrato('tony')

print('\n=================================')
print('TESTES NEGATIVOS (QA)')
print('=================================\n')

print('Teste N1 - Depósito com valor zero')
banco.depositar('tony', 0)

print('\nTeste N2 - Depósito negativo')
banco.depositar('tony', -100)

print('\nSaldo após depósitos inválidos')
banco.saldo_atual('tony')

print('\nTeste N3 - Saque acima do saldo')
banco.sacar('tony', 100000)

print('\nTeste N4 - Transferência para conta inexistente')
banco.transferir('tony', 'thor', 50)

print('\nTeste N5 - Consulta de conta inexistente (saldo)')
banco.saldo_atual('natasha')

print('\nTeste N6 - Consulta de conta inexistente (extrato)')
banco.mostrar_extrato('natasha')
```

---

## Sua Tarefa

- Criar pelo menos 3 Casos de Teste
- Registrar pelo menos 3 Bug Report profissional

---
