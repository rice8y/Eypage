# グラフの描画

Julia でグラフを描画する方法は複数あるが, ここでは Plots を扱う.

まずは, Plots をインストールする. 

```Julia
(@v1.9) pkg> add Plots
```

または,

```Julia
using Pkg
Pkg.add("Plots")
```

次に, バックエンドをインストールする. Plots で扱えるバックエンドは複数ある.

| バックエンド | 特徴 |
| ---- | ---- |
| GR | デフォルト, 高速 (個人的に一番好き...) |
| PyPlot | Python の Matplotlib を使用 |
| PythonPlot | Python の Matplotlib を使用 |
| Plotly/PlotlyJS | インタラクティブなグラフの描画が可能 |
| PGFPlotsX | LaTex の PGF/Tikz を使用して高品質なグラフの描画が可能 |
| UnicodePlots | ターミナルでグラフを描画 |
| InspectDR | 高速 |
| Gaston | Gnuplot を使用 |

```{warning}
PyPlot は非推奨となったため, PythonPlot に移行した方がよさそう.
```

例として, 
$$
\begin{equation*}
y=\sin x,\quad 0\leq x\leq2\pi
\end{equation*}
$$
を各バックグラウンドで描画する.

| GR |
| --- |
|<img src="images/gr.png" style="height:200px; width:auto;">|

| PythonPlot |
| --- |
|<img src="images/pythonplot.png" style="height:200px; width:auto;">|

| PlotlyJS |
| --- |
|<img src="images/plotlyjs.svg" style="height:200px; width:auto;">|

| PGFPlotsX |
| --- |
|<img src="images/pgfplotsx.png" style="height:200px; width:auto;">|

| UnicodePlots |
| --- |
|<img src="images/unicodeplots.png" style="height:200px; width:auto;">|

| InspectDR |
| --- |
|<img src="images/inspectdr.png" style="height:200px; width:auto;">|

| Gaston |
| --- |
|<img src="images/gaston.png" style="height:200px; width:auto;">|

プロット時間は以下の通り.

| バックエンド | 初回実行時間 [s] | 平均実行時間 [s] |
| --- | --- | --- |
| GR | 0.133777 | 0.000233 |
| PythonPlot | 0.920604 | 0.000226 |
| PlotlyJS | 0.760624 | 0.000220 |
| PGFPlotsX | 0.917138 | 0.000229 |
| UnicodePlots | 0.679810 | 0.000411 |
| InspectDR | 0.834717 | 0.000401 |
| Gaston | 0.755034 | 0.000493 |
