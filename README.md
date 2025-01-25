# Python e TDD: Testes unitários com unittest e pytest
Curso da Alura ensinando a utilizar o unittest para escrever e executar testes unitários utilizando o Python.

# Sumário
1. [TDD](#tdd)
2. [unittest](#unittest)
3. [Fixtures no unittest](#fixtures-no-unittest)
4. [pytest](#pytest)
5. [Fixtures no pytest](#fixtures-no-pytest)

# TDD
Test-Driven Development (TDD) é uma prática de desenvolvimento de software que consiste em escrever testes antes de implementar o código. O ciclo básico do TDD é composto por três etapas principais:

1. **Red (Falhar):** Escrever um teste para uma funcionalidade ainda não implementada e garantir que ele falhe.
2. **Green (Passar):** Implementar o código necessário para fazer o teste passar.
3. **Refactor (Refatorar):** Melhorar o código, garantindo que os testes ainda passem.

Essa abordagem ajuda a garantir que o código seja testável, reduz erros e promove um design de software mais limpo e eficiente.

[Sumário](#sumário)

# unittest
`unittest` ([unittest](https://docs.python.org/3/library/unittest.html)) é um módulo padrão do Python para escrever e executar testes unitários. Ele permite criar casos de teste, agrupar testes em classes e rodar esses testes de forma automatizada.

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

# Fixtures no unittest
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

# Pytest
## **Pytest: Uma Introdução**
O `pytest` ([pytest](https://docs.pytest.org/en/stable/)) é um dos frameworks de teste mais populares para Python, conhecido por sua simplicidade, flexibilidade e poder. Ele facilita a escrita de testes, incluindo testes unitários, de integração e de funcionalidade, com suporte a recursos avançados como fixtures reutilizáveis, parametrização de testes e suporte a plug-ins.

---

## **Instalação do Pytest**
Para instalar o `pytest`, você pode usar o gerenciador de pacotes `pip`. Siga os passos abaixo:

1. **Instalar via pip**:
   ```bash
   pip install pytest
   ```

2. **Verificar a instalação**:
   Após instalar, verifique a versão do `pytest`:
   ```bash
   pytest --version
   ```

3. **Configuração do Ambiente**:
   Crie um arquivo `requirements.txt` no seu projeto e inclua o `pytest`:
   ```
   pytest==<versão>
   ```
   Depois, instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```

4. **Executar testes**:
   Para rodar os testes, use o comando:
   ```bash
   pytest
   ```

---

## **Estrutura Básica de Testes com Pytest**
No `pytest`, os testes são identificados como funções que começam com o prefixo `test_`. Eles podem ser organizados em arquivos e pastas.

**Exemplo simples de teste:**

Crie um arquivo chamado `test_exemplo.py`:

```python
def soma(a, b):
    return a + b

def test_soma():
    assert soma(2, 3) == 5
    assert soma(-1, 1) == 0
```

Rode o comando `pytest` no terminal para executar o teste.

[Sumário](#sumário)

# Arquivo de configuração

O arquivo `pytest.ini` é um arquivo de configuração utilizado pelo `pytest` para personalizar e controlar o comportamento padrão dos testes em um projeto. Ele é especialmente útil para definir opções globais, ignorar warnings, configurar plugins e definir comportamentos específicos que serão aplicados em todas as execuções de teste no projeto.

---

### **Estrutura e Localização**

O arquivo `pytest.ini` deve ser colocado no diretório raiz do projeto. O `pytest` procura esse arquivo (ou outros arquivos de configuração suportados, como `tox.ini` ou `pyproject.toml`) para carregar as configurações antes de executar os testes.

---

### **Exemplo Básico**

Um arquivo `pytest.ini` típico pode se parecer com isto:

```ini
# pytest.ini
[pytest]
addopts = --maxfail=3 --tb=short
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
markers =
    slow: testes que são lentos
    api: testes relacionados à API
```

---

### **Principais Seções e Configurações**

1. **`addopts`**  
   - Adiciona opções de linha de comando padrão para o `pytest`.
   - Exemplo:  
     ```ini
     addopts = --maxfail=3 --disable-warnings
     ```
     Isso configura o `pytest` para parar após 3 falhas e ignorar warnings.

2. **`testpaths`**  
   - Define os diretórios onde o `pytest` buscará por testes.
   - Exemplo:  
     ```ini
     testpaths = tests integration_tests
     ```

3. **`python_files`**  
   - Define o padrão de nomes de arquivos que o `pytest` reconhece como arquivos de teste.
   - Padrão: `test_*.py` ou `*_test.py`.
   - Exemplo:  
     ```ini
     python_files = test_*.py *_spec.py
     ```

4. **`python_classes`**  
   - Define o padrão de nomes de classes que o `pytest` reconhece como classes de teste.
   - Padrão: Classes que começam com `Test`.
   - Exemplo:  
     ```ini
     python_classes = Test* Spec*
     ```

5. **`python_functions`**  
   - Define o padrão de nomes de funções que o `pytest` reconhece como funções de teste.
   - Padrão: Funções que começam com `test_`.
   - Exemplo:  
     ```ini
     python_functions = test_* check_*
     ```

6. **`markers`**  
   - Define marcadores personalizados para categorizar testes.
   - Exemplo:  
     ```ini
     markers =
         slow: testes que demoram muito tempo para serem executados
         api: testes relacionados à API
         database: testes que interagem com o banco de dados
     ```

7. **`filterwarnings`**  
   - Configura como warnings devem ser exibidos.
   - Exemplo:  
     ```ini
     filterwarnings =
         ignore::DeprecationWarning
         error::UserWarning
     ```

8. **Outras Configurações**  
   - Plugins do `pytest` podem adicionar opções ao `pytest.ini`. Por exemplo, o `pytest-cov` adiciona configurações para cobertura de código:
     ```ini
     [pytest]
     addopts = --cov=src --cov-report=html
     ```

---

### **Vantagens do Arquivo `pytest.ini`**

- **Centralização de Configurações**: Todas as opções padrão do `pytest` ficam definidas em um único lugar.
- **Redução de Repetição**: Evita passar as mesmas opções repetidamente na linha de comando.
- **Colaboração**: Outros desenvolvedores no projeto podem executar testes com as mesmas configurações.
- **Organização**: Permite categorizar e organizar testes com marcadores (`markers`).

---

### **Alternativas ao `pytest.ini`**

Além do `pytest.ini`, o `pytest` suporta outros arquivos de configuração, como:

1. **`tox.ini`**: Geralmente usado em projetos que utilizam `tox`.
2. **`pyproject.toml`**: Formato mais moderno, com suporte a várias ferramentas, incluindo o `pytest`.

---

O `pytest.ini` é uma ferramenta essencial para projetos que utilizam o `pytest`, ajudando a simplificar e padronizar a execução de testes, além de oferecer flexibilidade para lidar com configurações específicas do projeto.

[Sumário](#sumário)

# **Fixtures no Pytest**

As fixtures no `pytest` são funções usadas para preparar e fornecer dados ou recursos para os testes. Elas são definidas com o decorador `@pytest.fixture` e podem ser usadas em vários testes.

#### **Características Principais das Fixtures**
- Permitem o uso de dependências entre testes.
- Garantem a limpeza automática de recursos após o uso.
- Podem ser parametrizadas para gerar múltiplos cenários de teste.

#### **Exemplo Simples de Fixture**

```python
import pytest

@pytest.fixture
def usuario():
    return {"nome": "João", "idade": 30}

def test_usuario_nome(usuario):
    assert usuario["nome"] == "João"

def test_usuario_idade(usuario):
    assert usuario["idade"] == 30
```

#### **Escopo de uma Fixture**

O escopo controla quanto tempo uma fixture permanece ativa. Os valores possíveis são:
- `function` (padrão): Executada para cada teste.
- `class`: Executada uma vez por classe de teste.
- `module`: Executada uma vez por módulo.
- `session`: Executada uma vez por sessão de teste.

**Exemplo com escopo:**

```python
@pytest.fixture(scope="module")
def conexao_banco():
    conexao = criar_conexao()  # Função fictícia
    yield conexao  # Retorna a conexão para os testes
    conexao.close()  # Fecha a conexão após os testes
```

#### **Fixtures Dependentes**
Uma fixture pode depender de outra:

```python
@pytest.fixture
def db():
    return {"conectado": True}

@pytest.fixture
def usuario(db):
    return {"nome": "Maria", "db_status": db["conectado"]}

def test_usuario_com_db(usuario):
    assert usuario["db_status"] is True
```

#### **Parametrização de Fixtures**
Fixtures podem ser parametrizadas para gerar múltiplos cenários:

```python
@pytest.fixture(params=[1, 2, 3])
def numero(request):
    return request.param

def test_numero(numero):
    assert numero in [1, 2, 3]
```

---

### **Vantagens do Pytest**
1. **Simplicidade**: Sintaxe intuitiva e fácil de aprender.
2. **Extensibilidade**: Suporte a plugins e personalizações.
3. **Fixtures Poderosas**: Preparação e limpeza automáticas de recursos.
4. **Relatórios**: Saída detalhada dos testes no terminal.
5. **Parametrização**: Facilita a criação de cenários variados.

Com sua flexibilidade e recursos avançados, o `pytest` é amplamente utilizado tanto para projetos pequenos quanto para aplicações corporativas complexas.

[Sumário](#sumário)