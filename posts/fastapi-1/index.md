# FastAPI



FastAPI-文档
<!--more-->

# 安装

```bash
pip install fastapi
pip install uvicorn[standard]
```

# 调试

## 运行测试服务

```python
uvicorn main:app --reload
```

`uvicorn main:app` 命令含义如下:

- `main`：`main.py` 文件（一个 Python「模块」）。
- `app`：在 `main.py` 文件中通过 `app = FastAPI()` 创建的对象。
- `--reload`：让服务器在更新代码后重新启动。仅在开发时使用该选项。

# 接口文档

## Docs

```python
http://127.0.0.1:8000/docs
```

## Redoc

```python
http://127.0.0.1:8000/redoc
```

# 接口参数

## 初始化

```python
from fastapi import FastAPI
from typing import Optional # 可选参数
from pydantic import BaseModel # 使用模型
app = FastAPI()
```

## 路径参数

## 裸奔版

```python
@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```

## 参数类型验证

```python
@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

## 接口调用顺序

由于*路径操作*是按顺序依次运行的，需要确保路径 `/users/me` 声明在路径 `/users/{user_id}`否则，`/users/{user_id}` 的路径还将与 `/users/me` 相匹配，"认为"自己正在接收一个值为 `"me"` 的 `user_id` 参数。

```python
@app.get("/users/me")
async def read_user_me():
    return {"user_id": "the current user"}


@app.get("/users/{user_id}")
async def read_user(user_id: str):
    return {"user_id": user_id}
```

## 参数预设值

```python
from enum import Enum
from fastapi import FastAPI

# 继承于str,枚举元素只能为str类型
class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

app = FastAPI()

@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name == ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}
	# 通过value属性获得真实值
    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}
```

## 参数查询

```python
fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]
@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]

# http://127.0.0.1:8000/items/?skip=0&limit=10
```

## 可选参数

```python
@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Optional[str] = None):
    if q:
        return {"item_id": item_id, "q": q}
    return {"item_id": item_id}
# Optional[str] 其中可选参数的类型为str
```

## 多路径参数

```python
@app.get("/users/{user_id}/items/{item_id}")
async def read_user_item(
    user_id: int, item_id: str, q: Optional[str] = None, short: bool = False
):
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```


## 请求体+路径参数

**FastAPI** 将识别出与路径参数匹配的函数参数应**从路径中获取**，而声明为 Pydantic 模型的函数参数应**从请求体中获取**

```python
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

app = FastAPI()

@app.put("/items/{item_id}")
async def create_item(item_id: int, item: Item):
    return {"item_id": item_id, **item.dict()}
```

## 请求体 + 路径参数 + 查询参数

```python
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

app = FastAPI()

@app.put("/items/{item_id}")
async def create_item(item_id: int, item: Item, q: Optional[str] = None):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
    return result
```

## 使用Path接收参数

路径参数总是必需的，因为它必须是路径的一部分。

所以，你应该在声明时使用 `...` 将其标记为必需参数。

然而，即使你使用 `None` 声明路径参数或设置一个其他默认值也不会有任何影响，它依然会是必需参数

```python
from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    q: str, item_id: int = Path(..., title="The ID of the item to get")
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

## 参数列表

```python
@app.get("/items/")
# async def read_items(q: Optional[List[str]] = Query(None)):
async def read_items(q: List[str] = Query(["foo", "bar"])): # 默认值为["foo", "bar"]
    query_items = {"q": q}
    return query_items
# http://localhost:8000/items/?q=foo&q=bar
""" response
{
  "q": [
    "foo",
    "bar"
  ]
}
"""
```

# 参数校验

## 字符长度校验

```python
from typing import Optional
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items/")
# 最小长度为3, 最大长度为50 默认值为 "fixedquery"
async def read_items(q: Optional[str] = Query("fixedquery", min_length=3, max_length=50)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```



## 参数列表不检查类型

```python
@app.get("/items/")
async def read_items(q: list = Query([])):
    query_items = {"q": q}
    return query_items
```

## 增加接口描述

`title`和`description`

这些信息将包含在生成的 OpenAPI 模式中，并由文档用户界面和外部工具所使用

```python
@app.get("/items/")
async def read_items(
    q: Optional[str] = Query(
        None,
        title="Query string",
        description="Query string for the items to search in the database that have a good match",
        min_length=3,
    )
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

## 别名参数

假设你想要查询参数为 `item-query`。

```
http://127.0.0.1:8000/items/?item-query=foobaritems
```

但是 `item-query` 不是一个有效的 Python 变量名称。最接近的有效名称是 `item_query`。

但是你仍然要求它在 URL 中必须是 `item-query`...

这时你可以用 `alias` 参数声明一个别名，该别名将用于在 URL 中查找查询参数值：

```python
@app.get("/items/")
async def read_items(q: Optional[str] = Query(None, alias="item-query")):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```



## 数值校验

- `gt`：大于（`g`reater `t`han）
- `ge`：大于等于（`g`reater than or `e`qual）
- `lt`：小于（`l`ess `t`han）
- `le`：小于等于（`l`ess than or `e`qual）

```python
@app.get("/items/{item_id}")
async def read_items(
    *, item_id: int = Path(..., title="The ID of the item to get", ge=1), q: str
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

## 浮点数校验

```python
@app.get("/items/{item_id}")
async def read_items(
    *,
    item_id: int = Path(..., title="The ID of the item to get", ge=0, le=1000),
    q: str,
    size: float = Query(..., gt=0, lt=10.5)
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

## 在Pydantic内部校验

```python
class Item(BaseModel):
    name: str
    description: Optional[str] = Field(
        None, title="The description of the item", max_length=300
    )
    price: float = Field(..., gt=0, description="The price must be greater than zero")
    tax: Optional[float] = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item = Body(..., embed=True)):
    results = {"item_id": item_id, "item": item}
    return results
```
实际上，`Query`、`Path` 和其他你将在之后看到的类，创建的是由一个共同的 `Params` 类派生的子类的对象，该共同类本身又是 Pydantic 的 `FieldInfo` 类的子类。
Pydantic 的 `Field` 也会返回一个 `FieldInfo` 的实例。
`Body` 也直接返回 `FieldInfo` 的一个子类的对象。还有其他一些你之后会看到的类是 `Body` 类的子类。
请记住当你从 `fastapi` 导入 `Query`、`Path` 等对象时，他们实际上是返回特殊类的函数。
注意每个模型属性如何使用类型、默认值和 `Field` 在代码结构上和*路径操作函数*的参数是相同的，区别是用 `Field` 替换`Path`、`Query` 和 `Body`。
