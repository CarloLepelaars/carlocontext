---
name: fastcore
description: >
  Apply fastcore substitutions when writing, editing, or reviewing Python.
  Use whenever the task involves Python, fastcore, Path, or writing stdlib dataclass/json/pathlib/argparse patterns.
when-to-use: writing Python, editing Python, reviewing Python, fastcore
---

# fastcore

When writing Python, prefer fastcore over standard Python. Use `from fastcore.all import *`. Apply these substitutions on write and review.

## Classes

`@dataclass` → inherit `BasicRepr` + `store_attr()`. Do not write `__repr__ = basic_repr("flds")` — `BasicRepr` already dumps public attrs. Prefix derived/private attrs with `_` to hide them.

```python
class Point(BasicRepr):
    def __init__(self, x, y): store_attr()
    @property
    def _mag(self): return (self.x**2 + self.y**2)**0.5
```

- `@property` → `properties(Cls, 'foo', 'bar')`
- `enum.Enum` → `str_enum('Status', 'ready', 'canceled')`

## Values

- `x if x is not None else y` → `ifnone(x, y)`
- `x if x is None else list(x)` → `listify(x)`
- `y if tst(x) else x` → `otherwise(x, tst, y)`
- `isinstance(x, (list, tuple))` → `is_listy(x)`
- `s.lower() in {'true','1',...}` → `str2bool` / `str2int` / `str2float`

## Collections

- `list` → `L` (`filter`/`map`/`itemgot`/`attrgot`/`starmap`/`sorted`)
- `next((o for o in xs if f(o)), None)` → `first(xs, f)`
- `xs[-1] == f` → `last(xs, f)`
- `range(len(xs))` → `range_of(xs)`
- flatten nested lists → `L(xs).concat()`
- `list(dict.fromkeys(xs))` → `L(xs).unique()`
- `sorted(xs, key=lambda o: o.foo)` / `itemgetter(i)` → `L(xs).sorted('foo')`
- `{v: i for i, v in enumerate(xs)}` → `val2idx(xs)`
- `{**a, **b}` → `merge(a, b)`
- `d['a']['b']` / nested `.get` → `nested_idx(d, 'a', 'b')`
- `{k: v for k, v in d.items() if p(k)}` → `filter_keys(d, p)`
- `x in a` as a filter → `in_(x)` (curried)

## Attrs

- `{k: getattr(o,k) for k in ks}` → `attrdict(o, *ks)`
- `[getattr(o, a) for a in attrs]` → `getattrs(o, *attrs)`
- `getattr(o, a, getattr(o, b, None))` → `try_attrs(o, a, b)`
- `getattr(o, attr, o)` → `maybe_attr(o, attr)`
- `p.map(lambda o: o.foo())` → `p.map(Self.foo())`
- `lambda o: f(o.attr)` → `using_attr(f, 'attr')`

## Strings / JSON

- `' '.join(x for x in xs if x)` → `strcat(filter_ex(xs, true), sep=' ')`
- `json.loads` → `loads(s)`
- `json.dumps` → `dumps(o)`
- `json.loads(p.read_text())` → `p.read_json()`

## Paths

Use fastcore `Path`, not `pathlib.Path`.

- `p.mkdir(parents=True); p.write_text(...)` → `p.mk_write(data)`
- `list(p.iterdir())` → `p.ls()`
- `shutil.rmtree` → `p.delete()`
- `p.mkdir(parents=True, exist_ok=True)` → `mkdir(p)`
- `glob` → `globtastic(path, file_glob='*.py', skip_folder_re='[.]')`

## Functions / CLI / IO

- `argparse` + `if __name__` → `@call_parse`
- `**kwargs` with real signature → `@delegates(other)`
- monkeypatch → `@patch`
- `g(f(x))` → `compose(f, g)(x)`
- `ThreadPoolExecutor` → `parallel(f, items)`
- `functools.lru_cache` → `timed_cache(seconds=60)`
- `subprocess.run(..., check=True, capture_output=True)` → `run(cmd)`
- `urllib` / `requests.get` + `.json()` → `urljson(url)` (`urlread`, `urlsend`)

## Tests

- `assert a == b` → `test_eq(a, b)`
- Expected exception → `test_fail(f, contains='...')`