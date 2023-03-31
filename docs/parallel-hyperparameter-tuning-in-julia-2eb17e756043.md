# Julia 中的并行超参数调谐

> 原文：<https://medium.com/analytics-vidhya/parallel-hyperparameter-tuning-in-julia-2eb17e756043?source=collection_archive---------17----------------------->

在我之前的[帖子](https://iwasnothing.medium.com/use-oscillator-of-rsi-in-machine-learning-prediction-of-stock-return-8de6b9543315)中，我使用的超参数是通过包[超视](https://github.com/baggepinnen/Hyperopt.jl)找到的。用法非常简单，它只是利用不同的采样器在我指定的参数范围内循环。

# 软件包安装

```
Pkg.add("Hyperopt")
```

## 用法:

**参数** : *seqlen，Nh，delta，lr，mm，day0*

**目标函数** : *myObjective* ，将调用我的模型训练，并返回目标成本。客观成本可以是我们旨在最小化的培训损失。

```
using Hyperoptho = @hyperopt for i=20,
sampler = RandomSampler(), 
seqlen = StepRange(3, 5, 20),
Nh = StepRange(5,3, 20),
delta = StepRange(10,5, 25),
lr =  exp10.(LinRange(-4,-3,10)),
mm =  LinRange(0.75,0.95,5),
day0 = StepRange(5,3, 10)@show myObjective(etf,seqlen,Nh,lr,mm,day0,day0+delta,:reg,i)endbest_params, min_f = ho.minimizer, ho.minimum
```

# 并行执行

超参数调整通常需要很长时间。在 Julia 中，我们可以让它跨多个工作进程并行运行。下面是创建 4 个工人进程的代码。运行下面的代码后，使用函数" *nworkers()* "显示有 4 个工作进程。

```
using Distributed
addprocs(4)
```

为了让每个 worker 进程知道我们代码的所有引用，我首先把所有的模型训练代码放在模块" *OsRSIConv.jl* "中。下面的代码将生成模块及其目录和文件“Project.toml”。然后折射模块定义中的代码，放入它创建的文件夹:“[*OsRSIConv/src/OsRSIConv . JL*](https://github.com/iwasnothing/JuliaConvGRU/blob/main/OsRSIConv/src/OsRSIConv.jl)”。

```
Pkg.generate("OsRSIConv")
```

然后使用宏“ *@everywhere* ”要求每个工作进程安装所需的包，并加载模块“ *OsRSIConv.jl* ”。

```
[@everywhere](http://twitter.com/everywhere) include("installPkg.jl")
[@everywhere](http://twitter.com/everywhere) push!(LOAD_PATH, "OsRSIConv/");
[@everywhere](http://twitter.com/everywhere) using OsRSIConv
```

然后只需将宏“@hyperopt”改为“@phyperopt”，它将自动在所有可用的工作进程上运行循环。

```
ho = @phyperopt for i=20,
sampler = RandomSampler(), 
seqlen = StepRange(3, 5, 20),
Nh = StepRange(5,3, 20),
delta = StepRange(10,5, 25),
lr =  exp10.(LinRange(-4,-3,10)),
mm =  LinRange(0.75,0.95,5),
day0 = StepRange(5,3, 10)@show myObjective(etf,seqlen,Nh,lr,mm,day0,day0+delta,:reg,i)endbest_params, min_f = ho.minimizer, ho.minimum
```

使用 4 个工作进程，我获得了 2.3 倍的速度提升。

> 非平行远视:630.5 秒
> 
> 并行 phyperot:264.4 秒

# 附赠曲目

使用机器学习模型的好处之一是，我可以用不同股票的数据应用同一个模型进行训练。我可以在股票符号数组的循环中运行训练。

为了加快训练速度，我可以使用 Julia 并行计算功能非常轻松地并行运行它。

同样，我们需要添加和加载每个工作进程所需的所有包。然后只需使用函数“ *pmap* ”对数组中的每个符号应用训练函数“ *trainPredict* ”。

```
include("installPkg.jl")
tstart = time()
list = ["FB","AAPL","AMZN","GOOG","NFLX","SQ","MTCH","AYX","ROKU","TTD"]result = pmap(OsRSIConv.trainPredict,list)
print(result)
println(vcat(result))
tend=time()
et=tend-tstart
[@show](http://twitter.com/show) et
```

**结果**

> 顺序循环基线:398 秒
> 
> 地图(平行循环):187 秒

*来源:*[*parallel . ipynb*](https://github.com/iwasnothing/JuliaConvGRU/blob/main/parallel.ipynb)