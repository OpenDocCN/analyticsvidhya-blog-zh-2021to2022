# 在下拉列表中添加两个以上选项时出现“值错误:长度必须匹配才能比较”

> 原文：<https://medium.com/analytics-vidhya/valueerror-lengths-must-match-to-compare-when-adding-more-than-2-options-in-dropdown-3b4e0a5c77d4?source=collection_archive---------6----------------------->

我收到一个请求，要求解决这个人面临了几周的一个错误。

他使用 dash 创建了一个仪表板，并有一个下拉菜单用于在图表上显示各种数据。但是，当他在下拉菜单中添加 2 个或更多选项时，它没有反映在仪表板上，而是显示一个错误消息:
**“值错误:长度必须匹配才能比较”**

网上没有一个单一的解决方案，所以我开始检查哪里出了问题。

#不想看解释可以直接跳到代码。

就在那时，我注意到从下拉菜单中传递的输入必须是一个字符串，以便与 dataframe 中的数据进行比较。现在这对于缺省值来说工作得很好，因为它是一个字符串。但是当你在下拉框中添加另一个选项时，python 会自动将它转换成一个列表，这样它就会分崩离析。因为我们无法比较“列表”和“字符串”。因此出现了值误差。
现在我知道了错误到底是什么，我只需要找到一种方法来规避这个问题。

你要做的是:

1.  **当 multi = False:**
    这个错误通常不会在 multi 为 False 时遇到，但是万一你面对它这就是你需要做的。为了确保从下拉列表中传递的值是一个字符串，您只需从 python 自动进行类型转换的列表中提取第一个值，然后用与该值匹配的数据创建一个新的 dataframe。最后将这个数据帧(代码中的 price_value)传递给图形。

代码:

```
app.layout = html.Div([

    html.H1(children = 'Price',
            style = {
                'text-align': 'center',
                'color':'black'
            } 
    ),

    dcc.Dropdown(id='dropdown', options=[  
        {'label': i, 'value': i} for i in li_name],
## list of unique namesvalue=li_name[0],
        multi=True),dcc.Graph(id='price')
])[@app](http://twitter.com/app).callback(
    dash.dependencies.Output('price', 'figure'),
    [dash.dependencies.Input('dropdown', 'value')])def update_output(value):
    if type(value)!=str: val = value[0]
        price_value = data[data['name']==val]fig = px.line(price_value, x='date', y='percent',color='blue') 

    return fig
```

2.**当 multi = True:**
在这种情况下，您必须显示下拉框中添加的所有选项。因此，如果我们想要 multi=True，我们将与列表中的每个值匹配的数据存储在一个新的 dataframe 中，该列表是从 dropdown 单独传递的。然后将这个新创建的 dataframe(代码中的 price_value)传递给 figure。

代码:

```
app.layout = html.Div([

    html.H1(children = 'Price',
            style = {
                'text-align': 'center',
                'color':'black'
            } 
    ),

    dcc.Dropdown(id='dropdown', options=[  
        {'label': i, 'value': i} for i in li_name],
## list of unique names value=li_name[0],
        multi=True),dcc.Graph(id='price')
])[@app](http://twitter.com/app).callback(
    dash.dependencies.Output('price', 'figure'),
    [dash.dependencies.Input('dropdown', 'value')])def update_output(value):
    if type(value)!=str:
        price_value = data[data['name'].isin(value) ]
    else:
        price_value = data[data['name']==value]fig = px.line(price_value, x='date', y='percent',color='blue') 

    return fig
```

这就是你如何规避值错误，让你的 Dash 应用程序正常工作。

*   还有一件事，如果你得到一个重复的调用错误，只要再次运行你的 dash 应用程序，它就会得到解决。即来自 app = dash。破折号()

希望这对你有帮助。谢谢大家！

如果你对此有任何疑问，你可以在 Linkedin 上给我发消息:
[www.linkedin.com/in/shraddha-shekhar](http://www.linkedin.com/in/shraddha-shekhar)

![](img/641aa135ad37781d8de6a691c9f518ba.png)