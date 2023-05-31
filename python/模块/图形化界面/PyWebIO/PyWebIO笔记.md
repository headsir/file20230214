# 一、上传文件，页面显示

```
# # 上传文件 multiple 指定文件数量
# file = web.input.file_upload('请选择需要加载的数据', accept=['.xlsx', '.xls'], multiple=True)
# # 读取excel数据
# df = pd.read_excel(file[0]['content'])
# # heml页面显示
# web.output.put_html(df.head(10).to_html())
```

## 二、启动服务

```
# 通过局域网连接网页
# remote_access,否启用远程访问,reconnect_timeout=1 服务器异常停止可以自动启动,
# auto_open_webbrowser=False   开启服务时，是否自动打开浏览器。默认false
# max_payload_size 设置Web框架允许上传的最大文件大小
web.start_server(bmi, debug=True, port=1122, remote_access=True, auto_open_webbrowser=True, max_payload_size="2G")
```