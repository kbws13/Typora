安装Flask-WTF

```
pip install flask-wtf
```

# 配置

```python
app = Flask(__name__)
app.config['SECRET_KEY'] = 'hard to guess string'
```

`app.config`字典可用于存储Flask，扩展和应用自身的配置变量

# 表单类

```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired

class NameForm(FlaskForm):
    name = StringField('What is your name?', validators=[DataRequired()])
    submit = SubmitField('Submit')
```

WTFForms支持的HTML标准字段

![image-20230619094616055](4-Web%E8%A1%A8%E5%8D%95.assets/image-20230619094616055.png)

WTForns验证函数

![image-20230619094704405](4-Web%E8%A1%A8%E5%8D%95.assets/image-20230619094704405.png)

# 把表单渲染成HTML

视图函数提通过form参数把一个NameForm实例传入模板，在模板中可以生成一个简单的HTML表单

```
<form>
    {{form.hidden_tag()}}
    {{ form.name.label }} {{ form.name() }}
    {{form.submit}}
</form>
```

表单中的`form.hidden_tag()`元素，供Flask-WTF的CSRF防护机制使用

还可以为其指定id或css属性，然后为其定义CSS样式

```
<form>
    {{form.hidden_tag()}}
    {{ form.name.label }} {{ form.name(id='my-text-field') }}
    {{form.submit}}
</form>
```

使用Bootstrap预定义的表单样式渲染整个Flask-WTF表单

```
{% import "bootstrap/wtf.html" as wtf %}

{{ wtf.quick_form(form) }}
```

完整表单

```
{% extends "base.html" %}
{% import "bootstrap/wtf.html" as wtf %}

{% block title %}FlaskY{% endblock %}

{% block page_content %}
<div class="page-header">
    <h1>Hello, {% if name %}{{name}}{% else %}Stranger{% endif %}!</h1>
</div>
{{ wtf.quick_form(form) }}
{% endblock %}
```

# 在视图函数中处理表单

使用GET和POST请求方法处理web表单

```python
@app.route('/', methods=['GET', 'POST'])
def index():
    name = None
    form = NameForm()
    if form.validate_on_submit():
        name = form.name.data
        form.name.data = ''
    return render_template('index.html', form=form, name=name)
```

# 重定向和用户会话

```python
from flask import Flask, render_template, session, redirect, url_for

@app.route('/', methods=['GET', 'POST'])
def index():
    form = NameForm()
    if form.validate_on_submit():
        session['name'] = form.name.data
        return redirect(url_for('index'))
    return render_template('index.html', form=form, name=session.get('name'))
```

# 闪现消息

```py
from flask import Flask, render_template, session, redirect, url_for, flash

@app.route('/', methods=['GET', 'POST'])
def index():
    form = NameForm()
    if form.validate_on_submit():
        old_name = session.get('name')
        if old_name is not None and old_name != form.name.data:
            flash('Looks like you have changed your name!')
        session['name'] = form.name.data
        return redirect(url_for('index'))
    return render_template('index.html', form=form, name=session.get('name'))
```

base.html

```
{% block content %}
<div class="container">
    {% for message in get_flashed_messages() %}
    <div class="alert alert-warning">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        {{message}}
    </div>
    {% endfor %}

    {% block page_content %}{% endblock %}
</div>
{% endblock %}
```

