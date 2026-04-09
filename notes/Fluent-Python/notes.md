# Python Fluente — Notas de estudo

---

## Part I — Prólogo

### Capítulo 1 · O modelo de dados do Python
`8 de abril de 2026`

O capítulo introduz os **métodos especiais** (também chamados de *dunder methods* ou *magic methods*) e explica como eles permitem que classes criadas pelo usuário se comportem como tipos nativos do Python.

A ideia central é que o interpretador raramente chama esses métodos diretamente — quem os aciona são as funções e operadores built-in. Ao implementar um método especial em uma classe, você faz com que ela "fale a língua" do Python e se integre ao modelo de dados da linguagem.

#### `__repr__` vs `__str__`

Dois dos métodos especiais mais importantes são `__repr__` e `__str__`. Eles têm propósitos distintos:

| Método | Público-alvo | Acionado por |
|---|---|---|
| `__repr__` | Desenvolvedor | `repr()`, console interativo |
| `__str__` | Usuário final | `print()`, `str()` |

> **Regra de fallback:** se `__str__` não estiver definido, o `print()` usa `__repr__` como substituto. O contrário não ocorre.

A boa prática é que `__repr__` retorne uma string que idealmente permita **recriar o objeto**, enquanto `__str__` retorne uma representação legível para o usuário final.

#### Exemplo prático

```python
from math import hypot

class Vector:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def __repr__(self):
        return f'Vector({self.x!r}, {self.y!r})'  # para desenvolvedores

    def __str__(self):
        return f'({self.x}, {self.y})'             # para usuários

    def __abs__(self):
        return hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

v = Vector(2, 4)

print(repr(v))   # Vector(2, 4)  — usa __repr__
print(str(v))    # (2, 4)        — usa __str__
print(v)         # (2, 4)        — print() prefere __str__
```

**Experimento:** remova o `__str__` e rode novamente. O `print()` vai passar a usar `__repr__` como fallback — o que confirma que são métodos independentes, não encadeados.

#### Pontos-chave do capítulo

- Métodos especiais são acionados pelo interpretador, não chamados diretamente pelo seu código.
- Implementar `__repr__` é o mínimo para qualquer classe que precise ser inspecionada durante o desenvolvimento.
- `__str__` é opcional; se ausente, o Python usa `__repr__` como fallback no `print()`.
- Outros métodos especiais cobertos no capítulo: `__len__`, `__getitem__`, `__abs__`, `__bool__`, `__add__`, `__mul__`.

---