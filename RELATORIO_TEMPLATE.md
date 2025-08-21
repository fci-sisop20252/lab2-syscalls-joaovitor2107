# 📝 Relatório do Laboratório 2 - Chamadas de Sistema

---

## 1️⃣ Exercício 1a - Observação printf() vs 1b - write()

### 💻 Comandos executados:
```bash
strace -e write ./ex1a_printf
strace -e write ./ex1b_write
```

### 🔍 Análise

**1. Quantas syscalls write() cada programa gerou?**
- ex1a_printf: 9 syscalls
- ex1b_write: 7 syscalls

**2. Por que há diferença entre os dois métodos? Consulte o docs/printf_vs_write.md**

```
Essa diferença entre os dois métodos se dá pelo fato de que cada vez que o printf é chamado você não tem necessariamente uma chamada de syscall, você pode ter vários printf e uma só chamada e um só printf e mais de uma chamada de write. Nesse caso em específico temos que em cada uma das linhas de printf que contem dois \n, nós temos dois syscall e aqui temos duas assim, sendo o motivo da diferença do número de syscall de write.
```

**3. Qual método é mais previsível? Por quê você acha isso?**

```
O método mais previsível é o write(), apesar de ser mais lento na maioria dos casos aqui sabemos exatamente quantas syscalls serão feitas dado que cada chamada de write é uma syscall.
```

---

## 2️⃣ Exercício 2 - Leitura de Arquivo

### 📊 Resultados da execução:
- File descriptor: 3
- Bytes lidos: 127

### 🔧 Comando strace:
```bash
strace -e openat,read,close ./ex2_leitura
```

### 🔍 Análise

**1. Qual file descriptor foi usado? Por que não começou em 0, 1 ou 2?**

```
O file descriptor usado foi o 3, e não utilizamos o 0,1 ou 2 porque esses são reservados para entrada(scanf), saida(printf) e para saida de erro. Então temos o primeiro file descriptor após esse.
```

**2. Como você sabe que o arquivo foi lido completamente?**

```
Nos sabemos que o arquivo foi lido completamente quando o retorno da função read é 0
```

**3. Por que verificar retorno de cada syscall?**

```
Para podermos ter garantias do funcionamento das operações, caso tentemos ler um arquivo que não conseguimos abrir por exemplo poderemos ter erros e bugs inesperados e queremos evitar isso, pois estamos lidando com syscall que são executadas diretamente pelo kernel.
```

---

## 3️⃣ Exercício 3 - Contador com Loop

### 📋 Resultados (BUFFER_SIZE = 64):
- Linhas: 25 (esperado: 25)
- Caracteres: 1300
- Chamadas read(): 21 - mas no strace diz que são 23
- Tempo: 0.000101 segundos

### 🧪 Experimentos com buffer:

| Buffer Size | Chamadas read() | Tempo (s) |
|-------------|-----------------|-----------|
| 16          |                 |           |
| 64          |                 |           |
| 256         |                 |           |
| 1024        |                 |           |

### 🔍 Análise

**1. Como o tamanho do buffer afeta o número de syscalls?**

```
Quanto menor o buffer maior o número de syscalls necessárias para conseguir ler o arquivo, dado que cada syscall de read só consegue ler o tamanho do buffer, então se o buffer for menor que o arquivo demora mais. 
```

**2. Todas as chamadas read() retornaram BUFFER_SIZE bytes? Discorra brevemente sobre**

```
Nem todas, quando chegamos em EOF, por exemplo, read() retorna só a quantidade de bytes lidos até o EOF
```

**3. Qual é a relação entre syscalls e performance?**

```
[Sua análise aqui]
```

---

## 4️⃣ Exercício 4 - Cópia de Arquivo

### 📈 Resultados:
- Bytes copiados: _____
- Operações: _____
- Tempo: _____ segundos
- Throughput: _____ KB/s

### ✅ Verificação:
```bash
diff dados/origem.txt dados/destino.txt
```
Resultado: [ ] Idênticos [ ] Diferentes

### 🔍 Análise

**1. Por que devemos verificar que bytes_escritos == bytes_lidos?**

```
[Sua análise aqui]
```

**2. Que flags são essenciais no open() do destino?**

```
[Sua análise aqui]
```

**3. O número de reads e writes é igual? Por quê?**

```
[Sua análise aqui]
```

**4. Como você saberia se o disco ficou cheio?**

```
[Sua análise aqui]
```

**5. O que acontece se esquecer de fechar os arquivos?**

```
[Sua análise aqui]
```

---

## 🎯 Análise Geral

### 📖 Conceitos Fundamentais

**1. Como as syscalls demonstram a transição usuário → kernel?**

```
[Sua análise aqui]
```

**2. Qual é o seu entendimento sobre a importância dos file descriptors?**

```
[Sua análise aqui]
```

**3. Discorra sobre a relação entre o tamanho do buffer e performance:**

```
[Sua análise aqui]
```

### ⚡ Comparação de Performance

```bash
# Teste seu programa vs cp do sistema
time ./ex4_copia
time cp dados/origem.txt dados/destino_cp.txt
```

**Qual foi mais rápido?** _____

**Por que você acha que foi mais rápido?**

```
[Sua análise aqui]
```

---

## 📤 Entrega
Certifique-se de ter:
- [ ] Todos os códigos com TODOs completados
- [ ] Traces salvos em `traces/`
- [ ] Este relatório preenchido como `RELATORIO.md`

```bash
strace -e write -o traces/ex1a_trace.txt ./ex1a_printf
strace -e write -o traces/ex1b_trace.txt ./ex1b_write
strace -o traces/ex2_trace.txt ./ex2_leitura
strace -c -o traces/ex3_stats.txt ./ex3_contador
strace -o traces/ex4_trace.txt ./ex4_copia
```
# Bom trabalho!
