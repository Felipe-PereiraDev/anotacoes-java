# Lambdas

### Função lambda usando Interface

Para usando uma função lambda usando interface é necessário que a Interface tenha apenas um método abstrato.

Usa-se a anotação : 
`*@FunctionalInterface`
Permite que  Interface tenha apenas uma método (Função) - Nome : Interface funcional*

exemplo:

```java
package lambdas;

public interface Calculo {
    double executar(double a, double b);

}
```

Como implementar:

```java
package lambdas;

public class CalculoTeste2 {
    public static void main(String[] args) {
        Calculo soma = (a, b) -> {
            return a + b;
        };

        // o return é feito de forma implícita numa função lambda
        Notas media = (n1, n2) ->  (n1 + n2) / 2;

        /* outra forma:
        * Notas media = (n1, n2) -> {
        *  return (n1 + n2) / 2;
        * };
        */

        System.out.println(soma.executar(3, 4));

        System.out.println(media.getMedia(10, 5.5));
    }
}

```

O retorno é feito de forma implícita se tiver apenas uma sentença de código e sem um par de chave `{ }` numa função lambda.

exemplo:

```java
public interface Calculo {
    double executar(double a, double b);

}
public class CalculoTeste2 {
    public static void main(String[] args) {
				Notas media = (n1, n2) ->  (n1 + n2) / 2;
		}
```

## 1 - Interface funcional

- `BinaryOperator<Object>`  Representa uma operação em dois operandos do mesmo tipo, produzindo um resultado do mesmo tipo que os operandos. Para o caso em que os operandos e o resultado são todos de o mesmo tipo.
exemplo:

```java
public class CalculoTeste4 {
    public static void main(String[] args) {
        BinaryOperator<Double> calcular = (n1, n2) -> n1 * n2;
        System.out.println(calcular.apply(10.0, 20.0));
    }
}
```
| Método                                      | Descrição                                                                                   | Exemplo de Uso                                                                                                                                   |
|---------------------------------------------|---------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| `reduce(BinaryOperator<T> accumulator)`    | Aplica uma função acumuladora associativa para reduzir os elementos da `Stream` a um único valor. | ```java<br>Stream<Integer> numbers = Stream.of(1, 2, 3, 4);<br>Integer result = numbers.reduce((a, b) -> a + b).orElse(0);  // Soma dos elementos``` |
| `reduce(T identity, BinaryOperator<T> accumulator)` | Semelhante ao `reduce` anterior, mas com um valor inicial (identidade). | ```java<br>Stream<Integer> numbers = Stream.of(1, 2, 3, 4);<br>Integer result = numbers.reduce(0, (a, b) -> a + b);  // Soma com identidade inicial``` |
| `max(Comparator<? super T> comparator)`     | Retorna o maior elemento da `Stream`, de acordo com o `Comparator`. O `BinaryOperator` pode ser utilizado internamente para comparação. | ```java<br>Stream<Integer> numbers = Stream.of(1, 2, 3, 4);<br>Optional<Integer> max = numbers.reduce((a, b) -> a > b ? a : b);<br>max.ifPresent(System.out::println);``` |


---

- `Predicate<Object>` recebe um parâmetro e retorna um valor boolean
exemplo:

```java
public class PredicadoPredicate {
    public static void main(String[] args) {
        Produto p1 = new Produto("Notebook",2400, 0.20);
        Predicate<Produto> isCaro = 
        produto -> (produto.preco * (1 - produto.desconto)) > 1500;
        System.out.println(isCaro.test(p1));
    }
}
```
 Métodos da `Stream<T>` que usam `Predicate<T>`:
| Método                        | Descrição                                                    |
|--------------------------------|--------------------------------------------------------------|
| `filter(Predicate<T> predicate)` | Filtra elementos da `Stream` com base na condição.         |
| `allMatch(Predicate<T> predicate)` | Retorna `true` se **todos** os elementos satisfazem o `Predicate`. |
| `anyMatch(Predicate<T> predicate)` | Retorna `true` se **pelo menos um** elemento satisfaz o `Predicate`. |
| `noneMatch(Predicate<T> predicate)` | Retorna `true` se **nenhum** elemento satisfaz o `Predicate`. |
| `removeIf(Predicate<T> predicate)` | Remove elementos de uma `Collection` com base no `Predicate`. |


---

- `Consumer<Object>` recebe um parâmetro mas não tem retorno
exemplo:

```java
public class ConsumidorConsumer {
    public static void main(String[] args) {
        Produto p1 = new Produto("Notebook", 2400 , 0.7);
        Consumer<Produto> imprimir =
                v -> System.out.println("-------------------------------------\n" +
                        "Nome: "+ v.nome +
                        "\nPreço Original: " + v.preco +
                        "\nPreço com Desconto :" + v.preco * (1 - v.desconto));
        Produto p2 = new Produto("Caneta", 200, 0.1);
        Produto p3 = new Produto("Lápis", 21, 0.1);
        Produto p4  = new Produto("Carrinho", 1000, 0.1);
        Produto p5 = new Produto("Caneta", 12, 0.1);
        List<Produto> listaP = Arrays.asList(p1, p2, p3, p4, p5);
        listaP.forEach(imprimir);
        var listaIsBarato = listaP.stream()
						        .map(produto -> produto.desconto < 0.20).toList();
    }
}
```

---

- `Function<Object, ReturnObject>` Recebe um parâmetro e retorna um valor do tipo especificado

```java
package lambdas.InterfaceFuncional;

import java.util.function.Function;

public class FuncaoFunction {
    public static void main(String[] args) {
        Function<Integer, String> parOuImpar = n -> n % 2 == 0 ? "par" : "ìmpar";
        Function<String, String> msg = m -> "O Resultado é " + m;
        System.out.println(parOuImpar.apply(10));
        for (int i = 0; i < 10; i++) {
            System.out.println(parOuImpar.andThen(msg).apply(i));
        }
        /* Retorno:
		     * O resultado é par
        */
    }
}
```

---

- `Supplier<Object>` A interface `Supplier<T>` tem um método funcional chamado `get()`. O objetivo dessa interface é fornecer uma maneira de criar ou fornecer um valor do tipo `T` sem precisar de parâmetros. Em outras palavras, é uma função que não aceita argumentos e retorna um valor.
exemplo:

```java
Supplier<Person> personSupplier = () -> new Person("John", 30);
Person person = personSupplier.get();
System.out.println(person.getName());  // Saída: John
```

     Uso em Stream

```java
import java.util.stream.Stream;
import java.util.function.Supplier;

Supplier<Double> randomDoubleSupplier = () -> Math.random();

Stream.generate(randomDoubleSupplier)
      .limit(5)
      .forEach(System.out::println);
```

---

- `UnaryOperator<Object>` Ela é uma interface funcional que representa uma função que aplica uma operação a um único argumento e retorna um resultado do mesmo tipo. Em outras palavras, um `UnaryOperator` é um tipo específico de `Function` que opera no mesmo tipo de dado para entrada e saída.

```java
public class OperadorUnario {
    public static void main(String[] args) {
        UnaryOperator<Integer> maisDois = n -> n + 2;
        UnaryOperator<Integer> vezesDois = n -> n * 2;
        UnaryOperator<Integer> aoQuadrado = n -> n * n;
        System.out.println(maisDois.andThen(vezesDois).andThen(aoQuadrado).apply(0));
        // Resultado: 16
    }
}
```

- BinaryOperator
- BiFuntion<Object, Object, R_Object>

## 2 - Foreach

Exemplo de como usar o  `foreach()`

```java
public class Foreach {
    public static void main(String[] args) {
        List<String> aprovados = Arrays.asList("Ana", "Bia", "Mari", "Leo", "May");
        System.out.println(aprovados);
        aprovados.forEach(nome -> System.out.println(nome));
        // Referência a Método
        aprovados.forEach(System.out::println);
    }
}
```

### Method Reference

exemplos de referência a método:

```java
public class Nome {
    public static void getNome(String nome) {
        System.out.println("Ola meu nome é " + nome);
    }
}

public class Foreach {
    public static void main(String[] args) {
        List<String> aprovados = Arrays.asList("Ana", "Bia", "Mari", "Leo", "May");
        aprovados.forEach(Nome::getNome);
    }
}
```

Depois de `Nome::` vem o método que deseja usar.

o retorno será:

<aside>
💡 Olá meu nome é Ana

Olá meu nome é Bia

Olá meu nome é Mari

Olá meu nome é Leo

Olá meu nome é May

</aside>

## 3 - Composição de Função

### Predicate Composição:

```java
public class PredicadoCmposicao {
    public static void main(String[] args) {
        Predicate<Integer> isPar = n -> n % 2 == 0;
        Predicate<Integer> isTresDigitos =
                n -> n >= 100 && n <= 999;
        System.out.println(isPar.and(isTresDigitos).test(123));
        System.out.println(isPar.or(isTresDigitos).negate().test(100));
    }
}
```
