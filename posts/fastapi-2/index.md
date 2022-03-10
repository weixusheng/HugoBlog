# FastAPI-2


FastAPI-文档
<!--more-->

# 请求模型

```python
from pydantic import BaseModel
app = FastAPI()

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

@app.post("/items/")
async def create_item(item: Item):
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```

# 嵌套模型

## List

```python
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    categories: list = []

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

### 带有类型的List

```python
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    categories: List[str] = []

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

## Set

元素不重复的集合

## 嵌套

```python
class Image(BaseModel):
    url: str
    name: str

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    categories: Set[str] = []
    image: Optional[Image] = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

# 特殊类型的校验

使用`Pydantic`校验

例如在`Image`模型中有一个`url`字段,可以将它声明为Pydantic的`HttpUrl`而不是`str`

```python
from typing import Optional, Set
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()

class Image(BaseModel):
    url: HttpUrl
    name: str

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    categories: Set[str] = set()
    image: Optional[Image] = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

该字符串将被检查是否为有效的 URL，并在 JSON Schema / OpenAPI 文档中进行记录。

# 纯列表请求体

如果期望的 JSON 请求体的最外层是一个 JSON `array`（即 Python `list`），则可以在路径操作函数的参数中声明此类型，就像声明 Pydantic 模型一样：

```
images: List[Image]
```

例如：

```python
from typing import List
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()

class Image(BaseModel):
    url: HttpUrl
    name: str

@app.post("/images/multiple/")
async def create_multiple_images(images: List[Image]):
    return images
```

# 模式增加示例

## `Field` 的附加参数

在 `Field`, `Path`, `Query`, `Body` 和其他你之后将会看到的工厂函数，你可以为JSON 模式声明额外信息，你也可以通过给工厂函数传递其他的任意参数来给JSON 模式声明额外信息，比如增加 `example`:

```python
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI()

class Item(BaseModel):
    name: str = Field(..., example="Foo")
    description: Optional[str] = Field(None, example="A very nice Item")
    price: float = Field(..., example=35.4)
    tax: Optional[float] = Field(None, example=3.2)

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

# 额外数据类型

使用[`Pydantic`类型](https://pydantic-docs.helpmanual.io/usage/types/)

例如`UUID`, `datetime.datetime`, `frozenset`, `Decimal`等

```python
from datetime import datetime, time, timedelta
from typing import Optional
from uuid import UUID
from fastapi import Body, FastAPI

app = FastAPI()

@app.put("/items/{item_id}")
async def read_items(
    item_id: UUID,
    start_datetime: Optional[datetime] = Body(None),
    end_datetime: Optional[datetime] = Body(None),
    repeat_at: Optional[time] = Body(None),
    process_after: Optional[timedelta] = Body(None),
):
    start_process = start_datetime + process_after
    duration = end_datetime - start_process
    return {
        "item_id": item_id,
        "start_datetime": start_datetime,
        "end_datetime": end_datetime,
        "repeat_at": repeat_at,
        "process_after": process_after,
        "start_process": start_process,
        "duration": duration,
    }
```

# Cookies

可以像定义 `Query` 参数和 `Path` 参数一样来定义 `Cookie` 参数

```python
from typing import Optional
from fastapi import Cookie, FastAPI

app = FastAPI()

@app.get("/items/")
# 第一个值是参数的默认值，同时也可以传递所有验证参数或注释参数，来校验参数：
async def read_items(ads_id: Optional[str] = Cookie(None)):
    return {"ads_id": ads_id}
```

# Header

使用和`Path`, `Query` and `Cookie` 一样的结构定义 header 参数

```python
from typing import Optional
from fastapi import FastAPI, Header

app = FastAPI()

@app.get("/items/")
# 第一个值是参数的默认值，同时也可以传递所有验证参数或注释参数，来校验参数：
async def read_items(user_agent: Optional[str] = Header(None)):
    return {"User-Agent": user_agent}
```

## 符号 ' - ' 的自动转换

大多数标准的headers用 "连字符" 分隔，也称为 "减号" (`-`)。

但是像 `user-agent` 这样的变量在Python中是无效的。

因此, 默认情况下, `Header` 将把参数名称的字符从下划线 (`_`) 转换为连字符 (`-`) 来提取并记录 headers.

同时，HTTP headers 是大小写不敏感的，因此，因此可以使用标准Python样式(也称为 "snake_case")声明它们。

因此，您可以像通常在Python代码中那样使用 `user_agent` ，而不需要将首字母大写为 `User_Agent` 或类似的东西。

如果出于某些原因，你需要禁用下划线到连字符的自动转换，设置`Header`的参数 `convert_underscores` 为 `False`:

```python
from typing import Optional
from fastapi import FastAPI, Header

app = FastAPI()

@app.get("/items/")
async def read_items(
    strange_header: Optional[str] = Header(None, convert_underscores=False)
):
    return {"strange_header": strange_header}
```

# 响应模型

```python
from typing import List, Optional
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    categories: List[str] = []

@app.post("/items/", response_model=Item)
async def create_item(item: Item):
    return item
```

## 添加输出模型

相反，我们可以创建一个有明文密码的输入模型和一个没有明文密码的输出模型：

```python
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()

class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: Optional[str] = None

class UserOut(BaseModel):
    username: str
    email: EmailStr
    full_name: Optional[str] = None

@app.post("/user/", response_model=UserOut)
async def create_user(user: UserIn):
    return user
```

## `response_model_exclude_unset` 

设置路径操作装饰器的 `response_model_exclude_unset=True` 参数：

响应中将不会包含那些默认值，而是仅有实际设置的值

```python
from typing import List, Optional
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    # 设置默认值 10.5
    tax: float = 10.5
    categories: List[str] = []

items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}

@app.get("/items/{item_id}", response_model=Item, response_model_exclude_unset=True)
async def read_item(item_id: str):
    return items[item_id]

"""
因此，如果你向路径操作发送 ID 为 foo 的商品的请求，则响应（不包括默认值）将为：
{
    "name": "Foo",
    "price": 50.2
}
"""
```

## `response_model_include` 和 `response_model_exclude`

你还可以使用*路径操作装饰器*的 `response_model_include` 和 `response_model_exclude` 参数。

它们接收一个由属性名称 `str` 组成的 `set` 来包含（忽略其他的）或者排除（包含其他的）这些属性。

如果你**只有一个 Pydantic 模型**，并且想要从输出中移除一些数据，则可以使用这种快捷方法。

```python
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: float = 10.5

items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The Bar fighters", "price": 62, "tax": 20.2},
    "baz": {
        "name": "Baz",
        "description": "There goes my baz",
        "price": 50.2,
        "tax": 10.5,
    },
}

@app.get(
    "/items/{item_id}/name",
    response_model=Item,
    response_model_include={"name", "description"}
)
async def read_item_name(item_id: str):
    return items[item_id]

@app.get(
    "/items/{item_id}/public",
    response_model=Item,
    response_model_exclude={"tax"}
)
async def read_item_public_data(item_id: str):
    return items[item_id]
```

# 额外的模型

对用户模型来说,拥有多个相关的模型是很常见的,因为：

- **输入模型**需要拥有密码属性。
- **输出模型**不应该包含密码。
- **数据库模型**很可能需要保存密码的哈希值。

## 多模型处理

```python
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()

class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: Optional[str] = None

class UserOut(BaseModel):
    username: str
    email: EmailStr
    full_name: Optional[str] = None

class UserInDB(BaseModel):
    username: str
    hashed_password: str
    email: EmailStr
    full_name: Optional[str] = None

def fake_password_hasher(raw_password: str):
    return "supersecret" + raw_password

def fake_save_user(user_in: UserIn):
    hashed_password = fake_password_hasher(user_in.password)
    user_in_db = UserInDB(**user_in.dict(), hashed_password=hashed_password)
    print("User saved! ..not really")
    return user_in_db

@app.post("/user/", response_model=UserOut)
async def create_user(user_in: UserIn):
    user_saved = fake_save_user(user_in)
    return user_saved
```

## `user_in.dict()`

`user_in` 是一个 `UserIn` 类的 Pydantic 模型.

Pydantic 模型具有 `.dict（）` 方法，该方法返回一个拥有模型数据的 `dict`。

## 解包 `dict`

如果我们将 `user_dict` 这样的 `dict` 以 `**user_dict` 形式传递给一个函数（或类），Python将对其进行「解包」。它会将 `user_dict` 的键和值作为关键字参数直接传递。

```python
UserInDB(**user_dict)
```

会产生类似于以下的结果：

```python
UserInDB(
    username="john",
    password="secret",
    email="john.doe@example.com",
    full_name=None,
)
```

## 解包 `dict` 和额外关键字

然后添加额外的关键字参数 `hashed_password=hashed_password`，例如：

```python
UserInDB(**user_in.dict(), hashed_password=hashed_password)
```

...最终的结果如下：

```python
UserInDB(
    username = user_dict["username"],
    password = user_dict["password"],
    email = user_dict["email"],
    full_name = user_dict["full_name"],
    hashed_password = hashed_password,
)
```

## 模型的继承

```python
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()

class UserBase(BaseModel):
    username: str
    email: EmailStr
    full_name: Optional[str] = None

class UserIn(UserBase):
    password: str

class UserOut(UserBase):
    pass

class UserInDB(UserBase):
    hashed_password: str

def fake_password_hasher(raw_password: str):
    return "supersecret" + raw_password

def fake_save_user(user_in: UserIn):
    hashed_password = fake_password_hasher(user_in.password)
    user_in_db = UserInDB(**user_in.dict(), hashed_password=hashed_password)
    print("User saved! ..not really")
    return user_in_db

@app.post("/user/", response_model=UserOut)
async def create_user(user_in: UserIn):
    user_saved = fake_save_user(user_in)
    return user_saved
```

## 模型列表

你可以用同样的方式声明由对象列表构成的响应。

为此，请使用标准的 Python `typing.List`：

```python
from typing import List
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str

items = [
    {"name": "Foo", "description": "There comes my hero"},
    {"name": "Red", "description": "It's my aeroplane"},
]

@app.get("/items/", response_model=List[Item])
async def read_items():
    return items
```

## `dict` 构成的响应

你还可以使用一个任意的普通 `dict` 声明响应，仅声明键和值的类型，而不使用 Pydantic 模型。

如果你事先不知道有效的字段/属性名称（对于 Pydantic 模型是必需的），这将很有用。

在这种情况下，你可以使用 `typing.Dict`：

```python
from typing import Dict
from fastapi import FastAPI

app = FastAPI()

@app.get("/keyword-weights/", response_model=Dict[str, float])
async def read_keyword_weights():
    return {"foo": 2.3, "bar": 3.4}
```

# 响应状态码

```python
from fastapi import FastAPI
app = FastAPI()

@app.post("/items/", status_code=201)
async def create_item(name: str):
    return {"name": name}
```

# 接收表单数据

依赖安装

```bash
pip install python-multipart
```

对于接收的数据类型不是`JSON`而是表单`Form`

```python
from fastapi import FastAPI, Form

app = FastAPI()

@app.post("/login/")
async def login(username: str = Form(...), password: str = Form(...)):
    return {"username": username}
```

## 请求文件

```python
from fastapi import FastAPI, File, UploadFile

app = FastAPI()

@app.post("/files/")
async def create_file(file: bytes = File(...)):
    return {"file_size": len(file)}

@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile = File(...)):
    return {"filename": file.filename}
```

建议使用`UploadFile`而不使用`bytes`

`UploadFile` 与 `bytes` 相比有更多优势：

- 使用`spooled`文件：

  - 存储在内存的文件超出最大上限时，FastAPI 会把文件存入磁盘；

- 这种方式更适于处理图像、视频、二进制文件等大型文件，好处是不会占用所有内存；

- 可获取上传文件的元数据；

- 自带 `file-like``async` 接口；

- 暴露的 Python `SpooledTemporaryFile`对象，可直接传递给其他预期「file-like」对象的库。

### `UploadFile`

`UploadFile` 的属性如下：

- `filename`：上传文件名字符串（`str`），例如， `myimage.jpg`；
- `content_type`：内容类型（MIME 类型 / 媒体类型）字符串（`str`），例如，`image/jpeg`；
- `file`： `SpooledTemporaryFile`。其实就是 Python文件，可直接传递给其他预期 `file-like` 对象的函数或支持库。

`UploadFile` 支持以下 `async` 方法，（使用内部 `SpooledTemporaryFile`）可调用相应的文件方法。

- `write(data)`：把 `data` （`str` 或 `bytes`）写入文件；

- `read(size)`：按指定数量的字节或字符（`size` (`int`)）读取文件内容；

- `seek(offset)`：移动至文件`offset`（int）字节处的位置；

  - 例如，`await myfile.seek(0)` 移动到文件开头；
  - 执行 `await myfile.read()` 后，需再次读取已读取内容时，这种方法特别好用；

- `close()`：关闭文件。

因为上述方法都是 `async` 方法，要搭配「await」使用。

例如，在 `async` *路径操作函数* 内，要用以下方式读取文件内容：

```python
contents = await myfile.read()
```

在普通 `def` *路径操作函数* 内，则可以直接访问 `UploadFile.file`，例如：

```python
contents = myfile.file.read()
```

## 多文件上传

可用同一个「表单字段」发送含多个文件的「表单数据」。

上传多个文件时，要声明含 `bytes` 或 `UploadFile` 的列表（`List`）：

```python
from typing import List
from fastapi import FastAPI, File, UploadFile
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.post("/files/")
async def create_files(files: List[bytes] = File(...)):
    return {"file_sizes": [len(file) for file in files]}

@app.post("/uploadfiles/")
async def create_upload_files(files: List[UploadFile] = File(...)):
    return {"filenames": [file.filename for file in files]}

@app.get("/")
async def main():
    content = """
<body>
<form action="/files/" enctype="multipart/form-data" method="post">
<input name="files" type="file" multiple>
<input type="submit">
</form>
<form action="/uploadfiles/" enctype="multipart/form-data" method="post">
<input name="files" type="file" multiple>
<input type="submit">
</form>
</body>
    """
    return HTMLResponse(content=content)
```

## `File`和`Form`同时上传

```python
from fastapi import FastAPI, File, Form, UploadFile

app = FastAPI()

@app.post("/files/")
async def create_file(
    file: bytes = File(...), fileb: UploadFile = File(...), token: str = Form(...)
):
    return {
        "file_size": len(file),
        "token": token,
        "fileb_content_type": fileb.content_type,
    }
```

# 错误处理

## 使用 `HTTPException`

向客户端返回 HTTP 错误响应，可以使用 `HTTPException`。

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}


@app.get("/items/{item_id}")
async def read_item(item_id: str):
    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"item": items[item_id]}
```

## 覆盖 `HTTPException` 错误处理器

同理，也可以覆盖 `HTTPException` 处理器。

例如，只为错误返回纯文本响应，而不是返回 JSON 格式的内容：

```python
from fastapi import FastAPI, HTTPException
from fastapi.exceptions import RequestValidationError
from fastapi.responses import PlainTextResponse
from starlette.exceptions import HTTPException as StarletteHTTPException

app = FastAPI()

@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request, exc):
    return PlainTextResponse(str(exc.detail), status_code=exc.status_code)

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    return PlainTextResponse(str(exc), status_code=400)

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    if item_id == 3:
        raise HTTPException(status_code=418, detail="Nope! I don't like 3.")
    return {"item_id": item_id}
```



​		 

