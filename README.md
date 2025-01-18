# Python e TDD: Testes unitários com unittest
Curso da Alura ensinando a utilizar o unittest para escrever e executar testes unitários utilizando o Python.

# Sumário
1. [TDD](#tdd)
2. [unittest](#unittest)
3. [Fixtures](#fixtures)

# TDD
Test-Driven Development (TDD) é uma prática de desenvolvimento de software que consiste em escrever testes antes de implementar o código. O ciclo básico do TDD é composto por três etapas principais:

1. **Red (Falhar):** Escrever um teste para uma funcionalidade ainda não implementada e garantir que ele falhe.
2. **Green (Passar):** Implementar o código necessário para fazer o teste passar.
3. **Refactor (Refatorar):** Melhorar o código, garantindo que os testes ainda passem.

Essa abordagem ajuda a garantir que o código seja testável, reduz erros e promove um design de software mais limpo e eficiente.

[Sumário](#sumário)

# unittest
`unittest` é um módulo padrão do Python para escrever e executar testes unitários. Ele permite criar casos de teste, agrupar testes em classes e rodar esses testes de forma automatizada.

## Estrutura básica de um teste com unittest
```python
    import unittest

    # Código a ser testado
    def soma(a, b):
        return a + b

    # Classe de teste
    class TestSoma(unittest.TestCase):
        def test_soma_positivos(self):
            self.assertEqual(soma(2, 3), 5)

        def test_soma_negativos(self):
            self.assertEqual(soma(-1, -4), -5)

    if __name__ == "__main__":
        unittest.main()
```

## Comandos
- Para rodar os testes pelo terminal execute `python -m unittest arquivo_teste.py
`
- Descobrir e executar todos os testes no diretório atual: `python -m unittest discover
`

[Sumário](#sumário)

# Fixtures
As **fixtures** no `unittest` são métodos utilizados para configurar e limpar os recursos necessários antes e/ou após a execução dos testes. Elas permitem que você prepare o ambiente de teste (como inicializar objetos ou estabelecer conexões com recursos externos) e o limpe de maneira controlada, garantindo que cada teste seja executado em condições previsíveis.

---

## **Principais Métodos de Fixtures**

### 1. **`setUp`**
- Método executado antes de cada método de teste.
- Ideal para preparar o ambiente ou inicializar objetos reutilizáveis.
- Deve ser definido como `def setUp(self)` na classe de teste.

**Exemplo:**
```python
import unittest

class TesteComSetup(unittest.TestCase):
    def setUp(self):
        self.lista = [1, 2, 3]  # Inicialização de um recurso antes de cada teste

    def test_soma(self):
        self.assertEqual(sum(self.lista), 6)

    def test_tamanho(self):
        self.assertEqual(len(self.lista), 3)
```

---

### 2. **`tearDown`**
- Método executado após cada método de teste.
- Usado para liberar recursos ou limpar o ambiente.
- Definido como `def tearDown(self)` na classe de teste.

**Exemplo:**
```python
class TesteComTeardown(unittest.TestCase):
    def setUp(self):
        self.arquivo = open('teste.txt', 'w')  # Abrindo um arquivo

    def tearDown(self):
        self.arquivo.close()  # Fechando o arquivo após o teste

    def test_arquivo_aberto(self):
        self.assertFalse(self.arquivo.closed)  # Verifica que o arquivo está aberto
```

---

### 3. **`setUpClass` e `tearDownClass`**
- Executados uma vez antes e depois de todos os testes na classe.
- São métodos de classe, portanto devem ser definidos como `@classmethod`.

**`setUpClass`**: Ideal para inicializar recursos que serão compartilhados entre os testes.

**`tearDownClass`**: Usado para limpar recursos compartilhados.

**Exemplo:**
```python
class TesteComSetupClass(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        cls.recurso_compartilhado = "Recurso único"

    @classmethod
    def tearDownClass(cls):
        cls.recurso_compartilhado = None

    def test_recurso(self):
        self.assertEqual(self.recurso_compartilhado, "Recurso único")
```

---

### 4. **`setUpModule` e `tearDownModule`**
- Executados uma vez antes e depois de todos os testes no módulo (arquivo de teste).
- Não pertencem a uma classe de teste, mas sim ao módulo.

**Exemplo:**
```python
def setUpModule():
    print("Configuração do módulo")

def tearDownModule():
    print("Limpeza do módulo")
```

---

## **Quando Usar Fixtures?**

- Use `setUp` e `tearDown` para testes que requerem preparação e limpeza individual.
- Prefira `setUpClass` e `tearDownClass` para inicializar recursos caros que serão compartilhados entre os testes de uma mesma classe (como conexões com bancos de dados).
- Utilize `setUpModule` e `tearDownModule` quando a preparação envolver recursos no nível do arquivo de teste.

---

## **Vantagens das Fixtures**
1. **Reutilização**: Reduz duplicação de código de configuração em cada teste.
2. **Previsibilidade**: Garante que cada teste começa e termina com o mesmo estado inicial.
3. **Isolamento**: Ajuda a evitar efeitos colaterais entre testes.

As fixtures no `unittest` são uma ferramenta poderosa para manter os testes organizados e confiáveis, especialmente em projetos maiores.

[Sumário](#sumário)
