# Projeto Prático em Linguagem C — Sistema de Cadastro com Arquivos Texto

## Objetivo
Desenvolver, em linguagem C, um sistema de gerenciamento de registros utilizando **arquivos texto** para armazenamento persistente de dados.

O sistema deverá permitir ao usuário realizar operações de cadastro, consulta, alteração, exclusão lógica e listagem de registros por meio de um menu interativo no terminal.

---

# Enunciado do Projeto

Desenvolva um programa em linguagem C chamado:

## **Sistema de Gerenciamento de Produtos**

O sistema será responsável por armazenar informações de produtos em um arquivo texto.

Cada produto deverá possuir os seguintes campos:

```c
struct Produto {
    int codigo;
    char nome[50];
    float preco;
    int quantidade;
    char ativo;
};
```

### Descrição dos campos

- **codigo** → Identificador único do produto
- **nome** → Nome do produto
- **preco** → Valor unitário
- **quantidade** → Quantidade em estoque
- **ativo** → Indicador lógico:
  - `'A'` → Registro ativo
  - `'E'` → Registro excluído logicamente

Essa struct atende ao requisito de múltiplos tipos:

- `int`
- `char[]`
- `float`
- `char`

---

# Funcionalidades Obrigatórias

O sistema deverá apresentar o seguinte menu:

```text
1 - Cadastrar produto
2 - Alterar produto
3 - Excluir produto (logicamente)
4 - Listar produtos ativos
5 - Listar todos os produtos
6 - Buscar produto por código
0 - Sair
```

---

## 1. Cadastrar Produto

Permitir inserir um novo produto no arquivo.

### Regras:
- Não permitir código duplicado
- Gravar o registro no final do arquivo
- Todo novo registro deve iniciar com status `'A'`

---

## 2. Alterar Produto

Permitir alterar:

- nome
- preço
- quantidade

A busca deve ser feita pelo **código do produto**.

O registro deverá ser regravado no arquivo com os novos dados.

---

## 3. Excluir Produto (Exclusão Lógica)

Ao excluir:

- O registro **não poderá ser removido fisicamente**
- Apenas alterar o campo:

```c
ativo = 'E';
```

---

## 4. Listar Produtos Ativos

Exibir somente produtos com:

```c
ativo == 'A'
```

---

## 5. Listar Todos os Produtos

Exibir todos os registros, inclusive excluídos.

Mostrar o status:

- Ativo
- Excluído

---

## 6. Buscar Produto por Código

Solicitar o código e:

- Exibir os dados se encontrado
- Informar se o registro estiver excluído
- Informar se não existir

---

# Requisitos Técnicos Obrigatórios

O projeto deve obrigatoriamente utilizar:

- `fopen()`
- `fprintf()`
- `fscanf()` ou `fgets()`
- `rewind()`
- `fclose()`

---

# Modularização

O programa deve possuir funções separadas para:

- menu
- cadastrar
- alterar
- excluir
- listar
- buscar

---

# Persistência

Arquivo sugerido:

```text
produtos.txt
```

Formato sugerido:

```text
101;Mouse Gamer;89.90;12;A
102;Teclado Mecânico;250.00;5;A
103;Monitor 24;899.99;2;E
```

---

# Critérios de Avaliação

| Critério | Pontos |
|---------|-------|
| Estrutura correta | 2,0 |
| Manipulação de arquivos texto | 2,0 |
| Inclusão e persistência | 1,5 |
| Alteração | 1,5 |
| Exclusão lógica | 1,5 |
| Listagem e busca | 1,0 |
| Organização e modularização | 0,5 |

**Total: 10,0 pontos**

---

# Desafio Extra (Bônus +1,0)

Adicionar:

```text
7 - Compactar arquivo
```

Essa funcionalidade deverá copiar apenas registros ativos para um novo arquivo e substituir o original.
