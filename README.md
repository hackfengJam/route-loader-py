# RouteLoader (for Flask)

RouteLoader is an API managing tool for Flask, including：
1. API descriptions managing
2. Checking incoming request according to API descriptions
3. Generating API Documents according to API descriptions



## Quick Example

*Notice: For a full example, please see [demo.py](demo.py) and other related files.*

```python
# -*- coding: utf-8 -*-

from flask import Flask, Blueprint, request, render_template, jsonify
from routeloader import RouteLoader

##### Init RouteLoader #####

# Load API config file
API_CONFIGS = {
  "app": {
    "index": {
      "showInDoc": True,
      "name"     : "Flask app index",
      "method"   : "get",
      "url"      : "/",
      "response" : "html"
    },
    "doPost": {
      "showInDoc": True,
      "name"     : "Flask app POST example",
      "method"   : "post",
      "url"      : "/do_post",
      "response" : "json",
      "body": {
        "data": {
          "id": {
            "$name"      : "ID",
            "$isRequired": True,
            "$type"      : "int",
            "$example"   : 1
          }
        }
      }
    }
  }
}
route_loader = RouteLoader(API_CONFIGS)

##### Use RouteLoader on Flask app object #####

# Flask app object
app = Flask(__name__)

@route_loader.route(app, 'app.index')
def index():
    return render_template('index.html')

@route_loader.route(app, 'app.doPost')
def do_post():
    ret = request.get_data(as_text=True)

    return ret

##### API Documents #####
route_loader.create_doc(app, '/doc')
```



## File list

|                               File                               |   Type  |                            Description                             |
|------------------------------------------------------------------|---------|--------------------------------------------------------------------|
| [routeloader.py](routeloader.py)                                 | Core    | RouteLoader core code (Route loader)                               |
| [objectchecker.py](objectchecker.py)                             | Core    | RouteLoader core code (JSON checker)                               |
| [templates/api_docs.html](templates/api_docs.html)               | Core    | RouteLoader core code (API Document template                       |
| [api_configs.yaml](api_configs.yaml)                             | Example | API description file in YAML                                       |
| [demo.py](demo.py)                                               | Example | Flask project example code                                         |
| [my_middlewares.py](my_middlewares.py)                           | Example | Example middlewares for Flask                                      |
| [my_decorators.py](my_decorators.py)                             | Example | Example decorators for Flask                                       |
| [templates/\_base.html](templates/_base.html)                    | Example | Flask project example template (Base template)                     |
| [templates/\_macros.html](templates/_macros.html)                | Example | Flask project example template (Macros)                            |
| [templates/index.html](templates/index.html)                     | Example | Flask project example template (Flask app index template)          |
| [templates/my_module_index.html](templates/my_module_index.html) | Example | Flask project example template (MyModule blueprint index template) |