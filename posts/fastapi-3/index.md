# FastAPI-3



FastAPI-文档
<!--more-->

# 构建fastapi项目

## 文件层级

```python
.
├── app
│   ├── __init__.py
│   ├── main.py
│   ├── dependencies.py
│   └── routers
│   │   ├── __init__.py
│   │   ├── items.py
│   │   └── users.py
│   └── internal
│       ├── __init__.py
│       └── admin.py

```

## `_init_.py`文件的作用

### package的标识

如果没有该文件，该目录就不会认为是package

包有两种导入方式:

精确导入：

```python
from Root.Pack1 import Pack1Class

import Root.Pack1.Pack1Class
```

模糊导入：

```python
from Root.Pack1 import *
```

### 定义package中的__all__

模糊导入中的*中的模块是由__all__来定义的

```python
__all__ = ["Pack1Class","Pack1Class1"]
```

## `APIRouter`

假设专门用于处理用户逻辑的文件是位于 `/app/routers/users.py` 的子模块。

你希望将与用户相关的*路径操作*与其他代码分开，以使其井井有条。

但它仍然是同一 **FastAPI** 应用程序/web API 的一部分（它是同一「Python 包」的一部分）。

你可以使用 `APIRouter` 为该模块创建*路径操作*。

### 导入 `APIRouter`

你可以导入它并通过与 `FastAPI` 类相同的方式创建一个「实例」：

```python
from fastapi import APIRouter
router = APIRouter()

@router.get("/users/", tags=["users"])
async def read_users():
    return [{"username": "Rick"}, {"username": "Morty"}]

@router.get("/users/me", tags=["users"])
async def read_user_me():
    return {"username": "fakecurrentuser"}

@router.get("/users/{username}", tags=["users"])
async def read_user(username: str):
    return {"username": username}
```

## APIRouter的参数集成

假设你在位于 `app/routers/items.py` 的模块中还有专门用于处理应用程序中「项目」的端点。

你具有以下*路径操作*：

- `/items/`
- `/items/{item_id}`

这和 `app/routers/users.py` 的结构完全相同。

但是我们想变得更聪明并简化一些代码。

我们知道此模块中的所有*路径操作*都有相同的：

- 路径 `prefix`：`/items`。
- `tags`：（仅有一个 `items` 标签）。
- 额外的 `responses`。
- `dependencies`：它们都需要我们创建的 `X-Token` 依赖项。

因此，我们可以将其添加到 `APIRouter` 中，而不是将其添加到每个路径操作中。

```python
from fastapi import APIRouter, Depends, HTTPException
from ..dependencies import get_token_header

router = APIRouter(
    prefix="/items",
    tags=["items"],
    dependencies=[Depends(get_token_header)],
    responses={404: {"description": "Not found"}},
)

fake_items_db = {"plumbus": {"name": "Plumbus"}, "gun": {"name": "Portal Gun"}}

@router.get("/")
async def read_items():
    return fake_items_db

@router.get("/{item_id}")
async def read_item(item_id: str):
    if item_id not in fake_items_db:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"name": fake_items_db[item_id]["name"], "item_id": item_id}

@router.put(
    "/{item_id}",
    tags=["custom"],
    responses={403: {"description": "Operation forbidden"}},
)
async def update_item(item_id: str):
    if item_id != "plumbus":
        raise HTTPException(
            status_code=403, detail="You can only update the item: plumbus"
        )
    return {"item_id": item_id, "name": "The great Plumbus"}
```

## 添加一些自定义的 `tags`、`responses` 和 `dependencies`

我们不打算在每个*路径操作*中添加前缀 `/items` 或 `tags =["items"]`，因为我们将它们添加到了 `APIRouter` 中。

但是我们仍然可以添加*更多*将会应用于特定的*路径操作*的 `tags`，以及一些特定于该*路径操作*的额外 `responses`：

```python
from fastapi import APIRouter, Depends, HTTPException
from ..dependencies import get_token_header

router = APIRouter(
    prefix="/items",
    tags=["items"],
    dependencies=[Depends(get_token_header)],
    responses={404: {"description": "Not found"}},
)

fake_items_db = {"plumbus": {"name": "Plumbus"}, "gun": {"name": "Portal Gun"}}

@router.get("/")
async def read_items():
    return fake_items_db

@router.get("/{item_id}")
async def read_item(item_id: str):
    if item_id not in fake_items_db:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"name": fake_items_db[item_id]["name"], "item_id": item_id}

@router.put(
    "/{item_id}",
    tags=["custom"],
    responses={403: {"description": "Operation forbidden"}},
)
async def update_item(item_id: str):
    if item_id != "plumbus":
        raise HTTPException(
            status_code=403, detail="You can only update the item: plumbus"
        )
    return {"item_id": item_id, "name": "The great Plumbus"}
```

## `FastAPI` 主体

位于 `app/main.py` 的模块

这将是你的应用程序中将所有内容联结在一起的主文件。

并且由于你的大部分逻辑现在都存在于其自己的特定模块中，因此主文件的内容将非常简单。

### 导入 `FastAPI`[¶](https://fastapi.tiangolo.com/zh/tutorial/bigger-applications/#fastapi_1)

你可以像平常一样导入并创建一个 `FastAPI` 类。

我们甚至可以声明`全局依赖项`，它会和每个 `APIRouter` 的依赖项组合在一起：

```python
from fastapi import Depends, FastAPI
from .dependencies import get_query_token, get_token_header
from .internal import admin
from .routers import items, users

app = FastAPI(dependencies=[Depends(get_query_token)])

app.include_router(users.router)
app.include_router(items.router)
app.include_router(
    admin.router,
    prefix="/admin",
    tags=["admin"],
    dependencies=[Depends(get_token_header)],
    responses={418: {"description": "I'm a teapot"}},
)

@app.get("/")
async def root():
    return {"message": "Hello Bigger Applications!"}
```

# 注入依赖

## 示例

```python
from typing import Optional
from fastapi import Depends, FastAPI
app = FastAPI()

async def common_parameters(q: Optional[str] = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons

@app.get("/users/")
async def read_users(commons: dict = Depends(common_parameters)):
    return commons
```

依赖项函数的形式和结构与*路径操作函数*一样。

因此，可以把依赖项当作没有「装饰器」（即，没有 `@app.get("/some-path")` ）的路径操作函数。

依赖项可以返回各种内容。

本例中的依赖项预期接收如下参数：

- 类型为 `str` 的可选查询参数 `q`
- 类型为 `int` 的可选查询参数 `skip`，默认值是 `0`
- 类型为 `int` 的可选查询参数 `limit`，默认值是 `100`

然后，依赖项函数返回包含这些值的 `dict`。

**目的:**

接收到新的请求时，**FastAPI** 执行如下操作：

- 用正确的参数调用依赖项函数（「可依赖项」）
- 获取函数返回的结果
- 把函数返回的结果赋值给*路径操作函数*的参数

无需创建专门的类，并将之传递给 **FastAPI** 以进行「注册」或执行类似的操作

这样，只编写一次代码，**FastAPI** 就可以为多个*路径操作*共享这段代码 。

## 在路径操作装饰器中添加 `dependencies` 参数

*路径操作装饰器*支持可选参数 ~ `dependencies`。

该参数的值是由 `Depends()` 组成的 `list`：

```python
from fastapi import Depends, FastAPI, Header, HTTPException
app = FastAPI()

async def verify_token(x_token: str = Header(...)):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")

async def verify_key(x_key: str = Header(...)):
    if x_key != "fake-super-secret-key":
        raise HTTPException(status_code=400, detail="X-Key header invalid")
    return x_key

@app.get("/items/", dependencies=[Depends(verify_token), Depends(verify_key)])
async def read_items():
    return [{"item": "Foo"}, {"item": "Bar"}]
```



# 相对导入`import`

一个单点 `.`表示从当前层级寻找包

```python
from .dependencies import get_token_header
```

两个点 `..`表示从上一级寻找包

```python
from ..dependencies import get_token_header
```

三个点`...`表示上上一级
