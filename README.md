# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: [Ícaro Gabriel Pereira Carvalho](https://github.com/leonardo-minora)

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial com Best-Fit

**Configuração inicial:**
- **RAM:** 64 KB (contígua, inicialmente vazia)
- **Memória Virtual (Disco):** espaço ilimitado
- **Processos:**

| Processo | Tamanho (KB) |
|----------|--------------|
| P1       | 20           |
| P2       | 15           |
| P3       | 25           |
| P4       | 10           |
| P5       | 18           |
| **Total**| **88 KB**    |

---

### Alocação Passo a Passo

#### 1. P1 (20 KB)
- **Escolha:** menor espaço livre que caiba → início da RAM `[0–64]`.
- **Alocação:** `[0–20]` ocupado.
- **Livre:** `[20–64]` (44 KB).

#### 2. P2 (15 KB)
- **Escolha:** menor espaço livre que caiba → bloco `[20–64]` (44 KB).
- **Alocação:** `[20–35]` ocupado.
- **Livre:** `[35–64]` (29 KB).

#### 3. P3 (25 KB)
- **Escolha:** menor espaço livre que caiba → bloco `[35–64]` (29 KB).
- **Alocação:** `[35–60]` ocupado.
- **Livre:** `[60–64]` (4 KB) → **não comporta mais nenhum processo grande**.

#### 4. P4 (10 KB)
- **Escolha:** menor espaço livre que caiba → nenhum espaço suficiente na RAM (maior livre = 4 KB).
- **Ação:** enviado para **Memória Virtual (Disco)**.
- **Livre na RAM:** `[60–64]` (4 KB).

#### 5. P5 (18 KB)
- **Escolha:** menor espaço livre que caiba → nenhum espaço suficiente na RAM (maior livre = 4 KB).
- **Ação:** enviado para **Memória Virtual (Disco)**.

---

### Estado Final da Memória

**📌 RAM (64 KB)**  
```
[0–20]   → P1 (20 KB)  
[20–35]  → P2 (15 KB)  
[35–60]  → P3 (25 KB)  
[60–64]  → Livre (4 KB)
```

**📌 Memória Virtual (Disco)**  
```
P4 (10 KB)  
P5 (18 KB)
```

---

### 2. Simular Memória Virtual (Paginação)


Os processos que não puderam ser alocados totalmente na RAM serão armazenados no disco (Memória Virtual), divididos em **páginas**.

### Configuração
- **Tamanho da RAM:** 64 KB
- **Tamanho da Página:** 4 KB
- **Processos:**

| Processo | Tamanho (KB) | Nº de Páginas |
|----------|--------------|--------------|
| P1       | 20           | 5            |
| P2       | 15           | 4            |
| P3       | 25           | 7            |
| P4       | 10           | 3            |
| P5       | 18           | 5            |

---

### Alocação de Páginas (BEST-FIT + Paginação)

| Página | Processo | Localização |
|--------|----------|-------------|
| 0      | P1       | RAM         |
| 1      | P1       | RAM         |
| 2      | P1       | RAM         |
| 3      | P1       | RAM         |
| 4      | P1       | RAM         |
| 5      | P2       | RAM         |
| 6      | P2       | RAM         |
| 7      | P2       | RAM         |
| 8      | P2       | RAM         |
| 9      | P3       | RAM         |
| 10     | P3       | RAM         |
| 11     | P3       | RAM         |
| 12     | P3       | RAM         |
| 13     | P3       | RAM         |
| 14     | P3       | RAM         |
| 15     | P3       | RAM         |
| 16     | P4       | DISCO       |
| 17     | P4       | DISCO       |
| 18     | P4       | DISCO       |
| 19     | P5       | DISCO       |
| 20     | P5       | DISCO       |
| 21     | P5       | DISCO       |
| 22     | P5       | DISCO       |
| 23     | P5       | DISCO       |

---

### 3. Desfragmentação da RAM

### Situação Antes da Desfragmentação

**RAM atual:**

```
[0–20]   → P1 (20 KB)  
[20–35]  → P2 (15 KB)  
[35–60]  → P3 (25 KB)  
[60–64]  → Livre (4 KB)
```

- Espaço livre total: 4 KB
- Espaço contíguo livre maior: 4 KB

### Desfragmentação

Ao **compactar a memória**, os blocos ocupados são movidos para o início, eliminando fragmentações.

**Após desfragmentação:**

```
[0–20]   → P1 (20 KB)  
[20–35]  → P2 (15 KB)  
[35–60]  → P3 (25 KB)  
[60–64]  → Livre (4 KB)
```

- Nenhuma fragmentação encontrada. Memória já estava contígua.

### E se o Processo P2 for Finalizado?

**Após liberar P2 (15 KB):**

```
[0–20]   → P1 (20 KB)  
[20–35]  → Livre (15 KB)  
[35–60]  → P3 (25 KB)  
[60–64]  → Livre (4 KB)
```

**Após desfragmentação:**

```
[0–20]   → P1 (20 KB)  
[20–45]  → P3 (25 KB)  
[45–63]  → P5 (18 KB)  
[63–64]  → Livre (1 KB)
```

- Com 19 KB contíguos, conseguimos alocar **P5 (18 KB)**.

### Estado Final Após Desfragmentação e Realocação

**RAM:**

```
[0–20]   → P1 (20 KB)  
[20–45]  → P3 (25 KB)  
[45–63]  → P5 (18 KB)  
[63–64]  → Livre (1 KB)
```

**DISCO:**

```
P4 (10 KB)
```

### Tabela de Páginas Atualizada

| Página | Processo | Localização |
|--------|----------|-------------|
| 0–4    | P1       | RAM         |
| 5–11   | P3       | RAM         |
| 12–16  | P5       | RAM         |
| 17–19  | P4       | DISCO       |

---

 ### 4. Questões para Reflexão
 **1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**
 
 No cenário apresentado, o algoritmo best-fit mostrou-se razoavelmente eficiente, embora sua vantagem sobre outros algoritmos como first-fit ou worst-fit não tenha sido plenamente explorada devido à ausência de fragmentações significativas na alocação inicial. O best-fit tenta sempre alocar o processo no menor espaço disponível que o comporte, com o objetivo de reduzir fragmentações. No entanto, como os processos foram inseridos em sequência e todos encontraram espaços contíguos grandes o suficiente, tanto o best-fit quanto o first-fit teriam produzido resultados praticamente idênticos. O worst-fit, por sua vez, poderia desperdiçar mais memória, já que tende a escolher os maiores blocos, o que aumenta a chance de sobras grandes porém inutilizáveis para processos subsequentes.

 **2. Como a memória virtual evitou um deadlock?**

 A memória virtual teve papel fundamental ao permitir que processos que não cabiam na RAM fossem paginados para o disco, evitando assim um deadlock por falta de recursos. Sem memória virtual, o sistema poderia travar ou impedir o carregamento de processos essenciais, já que todos os processos dependem de espaço na RAM para execução. Ao mover parte dos dados para o disco, o sistema operacional conseguiu continuar operando normalmente, mesmo com a RAM excedida, garantindo que os processos não fossem descartados nem bloqueados indefinidamente por falta de espaço físico.

 **3. Qual o impacto da desfragmentação no desempenho do sistema?**

 Já a desfragmentação impacta diretamente o desempenho do sistema, tanto de forma positiva quanto negativa. Por um lado, ela melhora a eficiência da alocação de memória ao consolidar espaços livres, possibilitando a entrada de novos processos sem recorrer à memória virtual, o que reduz o tempo de acesso e aumenta o desempenho. Por outro lado, o processo de desfragmentação exige movimentação de dados na RAM, o que consome tempo de CPU e pode interromper temporariamente a execução de processos. Portanto, seu uso precisa ser bem avaliado: é vantajoso quando há fragmentação que impede novas alocações, mas seu custo computacional deve ser considerado, especialmente em sistemas com muitos processos ou com requisitos de tempo real.