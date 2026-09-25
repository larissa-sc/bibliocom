# Contrato da API

## As 9 decisões do contrato

|                                                     | Decisão                                                                                   |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Prefixo e versão                                    | `/api/`                                                                                   |
| Barra final nas rotas                               | NÃO                                                                                       |
| Convenção de nomes dos campos                       | `camelCase`                                                                               |
| Formato de datas                                    | ISO 8601                                                                                  |
| Formato de valores monetários                       | string decimal                                                                            |
| Paginação: estilo e tamanho padrão                  | uso de `count`, `next`, `previous` e `results` com 20 itens como tamanho padrão           |
| Como se filtra, ordena e busca                      | parâmetros query string com `?q=`, `?ordering=` e `?{tipoEx}=`                            |
| Formato do erro de validação e do erro de permissão | objeto JSON erro 400/ 401 para autenticação e 403 para permissão                          |
| Relações                                            | elacionamentos serão representados de forma aninhada; na escrita, serão informados por ID |

## Tabela de Recursos

| Recurso     | Método | Rota                              | O que faz                     | Sucesso   |
| ----------- | ------ | --------------------------------- | ----------------------------- | --------- |
| Obras       | GET    | `/api/obras/`                     | Listagem                      | 200       |
|             | POST   | `/api/obras/`                     | Cria                          | 201       |
|             | GET    | `/api/obras/{id}/`                | Detalha                       | 200       |
|             | PATCH  | `/api/obras/{id}/`                | Atualiza parcialmente         | 200       |
|             | DELETE | `/api/obras/{id}/`                | Remove                        | 204       |
| Exemplares  | GET    | `/api/obras/{id}/exemplares/`     | Exemplares da obra            | 200       |
| Empréstimos | GET    | `/api/emprestimos/`               | Lista (filtrada pelo usuário) | 200       |
|             | POST   | `/api/emprestimos/`               | Registra empréstimo           | 201       |
|             | POST   | `/api/emprestimos/{id}/devolver/` | Registra devolução            | 200       |
| Sessão      | POST   | `/api/auth/login/`                | Autentica                     | 200       |
|             | POST   | `/api/auth/logout/`               | Encerra sessão                | 204       |
|             | GET    | `/api/auth/eu/`                   | Usuário atual                 | 200 / 401 |

## Exemplo de resposta JSON

### Listagem

```json
{
  "count": 128,
  "next": "https://bibliocom.org/api/obras/?page=3",
  "previous": "https://bibliocom.org/api/obras/?page=1",
  "results": [
    {
      "id": 42,
      "titulo": "Dom Casmurro",
      "autor": {
        "id": 7,
        "nome": "Machado de Assis"
      },
      "ano_publicacao": 1899,
      "exemplares_total": 3,
      "exemplares_disponiveis": 1
    }
  ]
}
```

### Detalhes

```json
{
  "id": 42,
  "titulo": "Dom Casmurro",
  "autor": {
    "id": 7,
    "nome": "Machado de Assis"
  },
  "ano_publicacao": 1899,
  "exemplares_total": 3,
  "exemplares_disponiveis": 1
}
```

## Exemplos de erro

### Erro de validação - 400 Bad Request

```json
{
  "titulo": ["Este campo é obrigatório."],
  "isbn": ["O ISBN deve ter 10 ou 13 dígitos."]
}
```

### Usuário não autenticado — 401 Unauthorized

```json
{
  "detail": "Usuário não autenticado."
}
```

### Usuário sem permissão — 403 Forbidden

```json
{
  "detail": "Você não tem permissão para realizar esta operação."
}
```
