---
layout: post
title: Exercícios de LES 2017/1 IFES
---
As implementações e testes de todos os exercícios se encontram no meu repositório do [Github](https://github.com/igorbelo/exercicios-les/).

## Cálculo do Imposto de Renda
<script src="https://gist.github.com/igorbelo/a20bbf57919ee592b8720703af810e7f.js"></script>

O padrão estratégia visa definir uma família de algoritmos e que haja uma separação entre algoritmos parecidos (nesse caso cálculo de imposto) de quem os utiliza.

Perceba que a classe `Funcionario` (linha 8) recebe um atributo salário e é com base nisso que o código sabe qual estratégia de cálculo de imposto utilizar, independentemente de qualquer outro atributo. Com isso, caso existam novos funcionários apenas o salário será considerado não sendo preciso implementar novamente o cálculo de imposto.

## Calculadora Polonesa
<script src="https://gist.github.com/igorbelo/0c357c6197907129b0f918d6189095cf.js"></script>

Nesse exercício foi sugerida a implementação utilizando o padrão cadeia de responsabilidade, mas por conta da simplicidade do problema uma simples estrutura de pilha no modelo foi capaz de suprir as necessidades do problema.

A explicação do MVC (Model-View-Controller) no problema é bem simples, a view (representada pela classe `CalculadoraView` [linha 50]) é responsável por possuir o input do usuário, que nesse caso será a notação polonesa da calculadora (algo como 5 5 * 3 +). Essa é a única responsabilidade da view, prover uma interface para o usuário entrar com dados.

O controller é responsável por realizar a ligação entre o que o usuário digitou e a regra de negócio. Nesse caso a classe `CalculadoraController` (linha 30) interpreta o input da view e realiza as chamadas correspondentes ao modelo, que será responsável pela regra de negócio, que nesse caso é calcular um resultado.

O modelo possui a regra de negócio de uma calculadora polonesa, e na classe `CalculadoraModel` essas regras são disponibilizadas através dos métodos públicos que serão invocados unicamente pelo controller.

## Sistema de Navegação FooBar Company
