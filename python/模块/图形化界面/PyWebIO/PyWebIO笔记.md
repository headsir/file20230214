# 一、上传文件，页面显示

```
# # 上传文件 multiple 指定文件数量
# file = web.input.file_upload('请选择需要加载的数据', accept=['.xlsx', '.xls'], multiple=True)
# # 读取数据
# df = pd.read_excel(file[0]['content'])
# # 页面显示
# web.output.put_html(df.head(10).to_html())
```