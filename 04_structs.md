![imagem](image/prof-saulo-santos-linguagem-c-structs.png)
# 📘 Estruturas (Structs) em C

*(Agrupando diferentes tipos de dados em um único registro)*

------------------------------------------------------------------------

## 1️⃣ O que é uma Struct em C?

Um **array** guarda vários elementos, mas obrigatoriamente do **mesmo tipo**.  
Uma **struct** (estrutura) permite agrupar variáveis de **tipos diferentes** sob um mesmo nome. Cada variável dentro de uma struct é chamada de **membro** ou **campo**.

Representa um **registro** de um objeto ou entidade do mundo real.

------------------------------------------------------------------------

# 🔹 PARTE 1 --- DECLARAÇÃO E ACESSO BÁSICO

Um exemplo comum: cadastrar dados de uma pessoa.

------------------------------------------------------------------------

## 📌 Declaração Clássica

``` c
struct Pessoa {
    char nome[50];
    int idade;
    double altura;
};
```

Para declarar instâncias (variáveis) do tipo que acabamos de criar:

``` c
struct Pessoa p1;
```

------------------------------------------------------------------------

## 📌 Uso do `typedef` (Melhor Prática)

O `typedef` cria um "apelido", evitando a necessidade de digitar a palavra `struct` toda vez que você for declarar uma variável. Esta é a regra de ouro do C moderno.

``` c
typedef struct {
    char nome[50];
    int idade;
    double altura;
} Pessoa;

// Agora basta usar o tipo direto:
Pessoa p1;
```

------------------------------------------------------------------------

## 📌 Inicialização e Acesso (Operador `.`)

Usamos o operador `.` (ponto) para acessar cada membro interno de uma variável do tipo `struct`.

``` c
#include <stdio.h>
#include <string.h>
#include "pss.h" 

typedef struct {
    char nome[50];
    int idade;
    double altura;
} Pessoa;

int main() {
    Pessoa p1;

    // Inicialização direta:
    Pessoa p2 = {"Ana", 25, 1.68};

    // Atribuindo valores a p1:
    strncpy(p1.nome, "Ana", sizeof(p1.nome)); // Para strings, recomendamos o uso seguro com strncpy!
    p1.idade = 30;
    p1.altura = 1.75;

    // Acesso para leitura (printf):
    printf("Nome: %s\n", p1.nome);
    printf("Idade: %d\n", p1.idade);
    
    // Acesso para escrita via teclado (utilizando PSS):
    
    p2.idade = input_d("\nDigite a nova idade de Ana: ");
    
    return 0;
}
```

------------------------------------------------------------------------

# 🔹 PARTE 2 --- ARRAY DE STRUCTS

Podemos criar vetores em que cada posição do vetor guarda uma struct completa. É a forma mais simples de gerenciar múltiplos registros (cadastros).

``` c
#include <stdio.h>
#include "pss.h"

typedef struct {
    char matricula[10];
    double nota;
} Aluno;

int main() {
    Aluno turma[3]; // Vetor comportando 3 registros de aluno diferentes
    
    for(int i = 0; i < 3; i++) {
        input_s("Matricula: ", turma[i].matricula, sizeof(turma[i].matricula)); // PSS blinda a string
        turma[i].nota = input_lf("Digite a nota: "); // PSS resolve validação e conversões
    }
    
    printf("\n--- Relatorio da Turma ---\n");
    for(int i = 0; i < 3; i++) {
        printf("Matricula: %s | Nota: %.2lf\n", turma[i].matricula, turma[i].nota);
    }

    return 0;
}
```

------------------------------------------------------------------------

# 🔹 PARTE 3 --- STRUCT DENTRO DE STRUCT (ANINHAMENTO)

Um campo/membro de uma struct pode facilmente ser outra struct previamente declarada. C permite organizar os dados hierarquicamente.

``` c
#include <stdio.h>

typedef struct {
    int dia, mes, ano;
} Data;

typedef struct {
    char nome[50];
    Data nascimento; // Um campo que possui o formato de uma struct
} Funcionario;

int main() {
    Funcionario f1;
    
    // Acessando membros hierárquicos com múltiplos pontos '.'
    f1.nascimento.dia = 15;
    f1.nascimento.mes = 8;
    f1.nascimento.ano = 1995;
    
    printf("Nasceu no ano: %d\n", f1.nascimento.ano);
    return 0;
}
```

------------------------------------------------------------------------

# 🔹 PARTE 4 --- PASSANDO STRUCTS PARA FUNÇÕES

Igual ao discutido no capítulo anterior, podemos passar parâmetros por *valor* ou por *referência*.

## 📌 Passagem por Valor (Cópia Inteira)
Ao passar uma struct por valor, **todo o conteúdo é copiado membro a membro**. Modificações na função não afetam a original. Além disso, se a struct for muito "pesada" (muitos membros ou vetores compridos dentro), esse método torna o código lento por gastar muito trabalho de CPU operando a cópia.

``` c
void imprimirPessoa(Pessoa p) {
    printf("Nome: %s, Idade: %d\n", p.nome, p.idade);
}
```

## 📌 Passagem por Referência (Ponteiro) - O Operador `->`
Esta é a **forma recomendada**. Passamos apenas endereço da estrutura (custo de poucos bytes na memória do sistema).
Para acessar membros de uma struct indiretamente usando um **ponteiro**, a linguagem C inventou o operador `->` (seta), que substitui magicamente o `.` (ponto).

``` c
#include <stdio.h>

typedef struct {
    char nome[50];
    int idade;
} Pessoa;

// Recebe ponteiro (*) e manipula interna usando seta (->)
void fazerAniversario(Pessoa *p) {
    p->idade++; 
    // Nota cultural: 'p->idade' é um atalho sintático em C para '(*p).idade'
}

int main() {
    Pessoa p1 = {"Joao", 35};
    fazerAniversario(&p1); // Passa o endereço usando &

    printf("Nova idade: %d\n", p1.idade); // Vai imprimir 36
    return 0;
}
```

------------------------------------------------------------------------

# 🔹 Erros Comuns

❌ **Tentar comparar structs inteiras com aspas de igual:** Fazer algo como `if(p1 == p2)` não é permitido pela linguagem C. Você tem que comparar campo por campo.\
❌ **Esquecer o limite do buffer na leitura (PSS):** Ao usar `input_s("Msg:", p.nome, sizeof(p.nome))` no novo framework, certifique-se de passar corretamente o tamanho usando `sizeof`.\
❌ **Usar `=` para strings:** Atribuir diretamente um nome a um vetor de char (`p1.nome = "Joao"`). Sempre devemos usar `strncpy` em arrays estáticos de C.\
❌ **Confundir os operadores (`.` x `->`):** Usar o famoso `.` (ponto) quando o parâmetro na sua função é um ponteiro de struct (deva-se usar `->`).

------------------------------------------------------------------------

# 🔹 Resumo Final

  ----------------------------------------------------------------------------
  Conceito                 Uso                                Exemplo
  ------------------------ ---------------------------------- ----------------
  **Operador `.`**         Acessar membros da variável local  `p1.idade`
  **Operador `->`**        Acessar membros vindo de ponteiros `ptr_p->idade`
  **Array de Structs**     Agrupar registros equivalentes     `turma[i].nota`
  **Struct Aninhada**      Struct engolindo outra Struct      `f1.data.ano`
  `typedef struct`         Evitar digitar o comando struct    `Pessoa p1;`
  ----------------------------------------------------------------------------

------------------------------------------------------------------------

# 🔹 Exercícios Resolvidos

## Exercício 1 -- Criação e Atribuição

Crie de forma elegante uma struct enumerada de `Livro` com os campos: `titulo`, `autor` e `paginas`. Feito a modelagem, instancie um livro localmente e em seguida exiba-o.

## ✅ Solução

``` c
#include <stdio.h>
#include <string.h>

typedef struct {
    char titulo[100];
    char autor[50];
    int paginas;
} Livro;

int main() {
    Livro l1;
    strncpy(l1.titulo, "O Senhor dos Aneis", sizeof(l1.titulo));
    strncpy(l1.autor, "J.R.R. Tolkien", sizeof(l1.autor));
    l1.paginas = 1200;

    printf("Livro: %s\nAutor: %s\nPaginas: %d\n", l1.titulo, l1.autor, l1.paginas);
    return 0;
}
```

------------------------------------------------------------------------

## Exercício 2 -- Função de Alta Performance (->)

Crie uma modelagem base do tipo Conta contendo número e saldo. Logo em seguida monte uma função em C que receba uma conta bancária sem custeio de processamento na memória do disco (vulgarmente conhecido como: através de referência/ponteiro) e adicione o valor de um depósito à variável saldo real.

## ✅ Solução

``` c
#include <stdio.h>

typedef struct {
    int numero_conta;
    double saldo;
} Conta;

void depositar(Conta *c, double valor) {
    c->saldo += valor;
}

int main() {
    Conta c1 = {1001, 500.00};
    depositar(&c1, 150.50);
    printf("Novo saldo garantido pelo Banco: R$ %.2lf", c1.saldo); 
    return 0; // Saldo exibirá: 650.50
}
```

------------------------------------------------------------------------

# Lista de Exercícios

## Estruturas (Structs) e Suas Aplicações em Linguagem C

### (Versão para Alunos -- Sem Soluções)

------------------------------------------------------------------------

1. **Cadastro Simples:** Crie uma struct chamada `Aluno` com os membros: `nome` (string), `idade` (int) e `curso` (string). No `main()`, declare uma variável desse tipo, peça para o usuário preencher os dados utilizando a biblioteca PSS (`input_s`, `input_d`, etc) e, ao final, os imprima formatados em tela.

2. **Lista de Produtos (Array de Structs):** Crie uma struct `Produto` contendo `nome` e `preco` (double). Na main, crie um vetor (array) livre para armazenar 3 instâncias de produtos. Use um laço de repetição (`for`) para ler sequencialmente os dados do terminal. Ao final, monte outro laço para exibir apenas o descritivo de produtos que custam estritamente menos que R$ 50,00.

3. **Struct Aninhada:** Crie uma struct limpa `Data` com os campos `dia`, `mes` e `ano`. Em seguida, crie em cima dela uma struct `Pessoa` contendo `nome` e aloque um campo chamado `nascimento` originário do seu modelo `Data`. Instancie uma pessoa, leia os dados numéricos e textuais de suas estruturas internas e imprima o resultado na máscara `"Fulano nasceu em DD/MM/AAAA"`.

4. **Passagem por Referência (`->`):** Crie uma clássica struct bidimensional `Retangulo` com `base` e `altura` (ambas double). Crie acima da `main` a função imperativa `void dobrarLados(Retangulo *r)` com a exclusiva premissa de multiplicar individualmente seus lados internos por 2 (Dica bônus: usar o operador `->`). O fluxo de execução do seu script criará o objeto físico real no bloco base e acoplará diretamente ele à chamada da sua função. Exiba os cálculos modificados.
