# ARCH — Refatoração Arquitetural

## Estrutura final

```text
lib/
 ├── core/
 │    ├── network/
 │    └── storage/
 │
 └── features/
      └── todos/
           ├── data/
           │    ├── datasources/
           │    │     todo_local_datasource.dart
           │    │     todo_remote_datasource.dart
           │    ├── models/
           │    │     todo_model.dart
           │    └── repositories/
           │          todo_repository_impl.dart
           │
           ├── domain/
           │    ├── entities/
           │    │     todo.dart
           │    └── repositories/
           │
           └── presentation/
                ├── pages/
                │     app_root.dart
                │     todos_page.dart
                ├── viewmodels/
                │     todo_viewmodel.dart
                └── widgets/
                      add_todo_dialog.dart
                      app_errors.dart
```

---

## Fluxo de dependências

```text
UI -> ViewModel -> Repository -> DataSources -> API / Storage
```

* A **UI** apenas exibe dados e dispara ações do usuário.
* O **ViewModel** controla o estado da tela e chama o Repository.
* O **Repository** decide de onde os dados vêm.
* Os **DataSources** acessam API (remoto) ou armazenamento local.

---

## Decisões

### ✔ Onde ficou a validação?

A validação de entrada (ex.: título vazio) ficou no **ViewModel**,
pois ele é responsável por controlar o estado e regras da interação da tela.

---

### ✔ Onde ficou o parsing JSON?

O parsing foi isolado na camada **data/models** (`TodoModel`),
responsável por converter JSON ⇄ entidade de domínio.

Isso evita que UI ou Domain conheçam detalhes da API.

---

### ✔ Como você tratou erros?

Os erros são capturados no **ViewModel** com `try/catch`,
que transforma exceções em mensagens de estado (`errorMessage`)
para a UI exibir sem conhecer a origem do erro.

---

## Resultado da Refatoração

* Separação clara entre apresentação, domínio e dados.
* UI não acessa HTTP nem armazenamento diretamente.
* Código preparado para crescer com novas features.
* Estrutura baseada em **feature-first**, melhorando manutenção e escalabilidade.
