# 1 SpringMVC

## 1.1 MVC

- Model（模型）
  - 业务逻辑

- View（视图）
  - 展示数据

- Controller（控制器）
  - 连接Model和View

理解：用户请求由Controller接收，由Controller调用相应的Model（业务逻辑），Model返回数据后，Controller将数据交由View（前端展示页面）渲染，最后展现给用户；Controller也可直接返回一个静态页面不需要Model介入。