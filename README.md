# visualisation-visdom
Making plots using visdom


## Heatmaps
```
import torchnet as tnt
confusion_matrix = tnt.meter.ConfusionMeter(3) # 3 classes 
y_actu = np.array([2, 2, 1, 2, 0, 2, 1])
y_pred = np.array([2, 2, 1, 2, 0, 2, 1])
confusion_matrix.add((torch.from_numpy(y_actu)),(torch.from_numpy(y_pred)))
print(confusion_matrix.value)

vis = visdom.Visdom(port='6002')
vis.heatmap(
        X=confusion_matrix.value(),
        opts=dict(
            columnnames=['a', 'b', 'c'],
            rownames=['a', 'b', 'c'],
            colormap='Electric',
        )
    )
```

<img src="https://github.com/ninjakx/visualisation-visdom/blob/master/Images/heatmap.png" width="300" height="300">


## Line(Custom)
  ```
  X = [[] for i in range(7)]
  Y = [[] for i in range(7)]
  trace=[]
  colors = ["red","blue","yellow","green","orange","violet","brown"]
  symbols = [5, 1, 31, 104, 126, 21, 27]
  m=[1,2,3,4,5,6,7]
  n =[8,9,10,11,12,13,14]
  
  c = -1
  for j in range(7):
      print()
      c=c+1
      X[j].append(m[c])
      Y[j].append(n[c])
      tr = dict(x=X[j], y=Y[j], marker={'color': colors[j], 'symbol': symbols[j], 'size': "10"},
                          mode="markers+lines",
                         name='class-'+str(j), type="custom")
      trace.append(tr)
    layout=dict(title="XY---Curve", xaxis={'title':'X'}, yaxis={'title':'Y'})

  vis._send({'data': trace, 'layout': layout, 'win': 'XYCURVE','update':'append'})
  ```
  <img src="https://github.com/ninjakx/visualisation-visdom/blob/master/Images/line_trace.PNG" width="500" height="500">
  
### OR

```
  trace1 = dict(x=[1,3,5,7], y=[1,3,5,7], marker={'color': 'red', 'symbol': 5, 'size': "10"},
                          mode="markers+lines",
                         name='class-1', type="custom")
  
  trace2 = dict(x=[2,4,6,8], y=[2,3,4,5], marker={'color': 'blue', 'symbol': 1, 'size': "10"},
                          mode="markers+lines", 
                         name='class-2', type="custom")
  
  trace3 = dict(x=[1,2,3,4], y=[4,3,2,1], marker={'color': 'green', 'symbol': 31, 'size': "10"},
                          mode="markers+lines", 
                         name='class-3', type="custom")
  
  trace4 = dict(x=[5,3,2,1], y=[1,2,3,6], marker={'color': 'orange', 'symbol': 104, 'size': "10"},
                          mode="markers+lines",
                         name='class-4', type="custom")
  
  trace5 = dict(x=[3,6,9,10], y=[3,2,1,0], marker={'color': 'violet', 'symbol': 126, 'size': "10"},
                          mode="markers+lines",
                         name='class-5', type="custom")
  
  trace6 = dict(x=[1,2,3,4], y=[1,4,7,8], marker={'color': 'brown', 'symbol': 21, 'size': "10"},
                          mode="markers+lines",
                         name='class-6', type="custom")
  
  trace7 = dict(x=[1,2,6,8], y=[1,4,3,7], marker={'color': 'yellow', 'symbol': 27, 'size': "10"},
                          mode="markers+lines",
                         name='class-7', type="custom")

  layout=dict(title="XY---Curve", xaxis={'title':'X'}, yaxis={'title':'Y'})

vis._send({'data': [trace1, trace2, trace3, trace4, trace5, trace6, trace7], 'layout': layout, 'win': 'XYCURVE','update':'append'})
```
<img src="https://github.com/ninjakx/visualisation-visdom/blob/master/Images/trace_loop.PNG" width="500" height="500">

```
vis_line = vis.line(X = np.array([1,2,3]), Y = np.array([[1,1,1],[2,2,4],[3,4,7]]), win="my_line")
```
![Line](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/my_line.PNG)


## Multiple Lines

```
def plot_combine(name,d):
        #index = {}
        #multiple plots in one single graph

        X = []
        Y = []
        legend = []
        for k, v in sorted(d.items()):
            print(k,v[0],v[1])
            Y.append(v[0])
            X.append(v[1])
            legend.append(k)
        Y = np.array([Y])
        X = np.array([X])
        vis.line(
            Y=Y, 
            X=X,
            win=name,
            env= 'main',
            opts=dict(
                title=name,
                legend=legend
            ),
            update= 'append',
        )
for i in [1,2,3,4,5,6]:
  d = {'class-1': [i+1,i*3] ,'class-2': [i*4,i+2]}
  plot_combine("combined_Lines",d)     
```
![Combined_lines](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/combined_lines.PNG)

## Multiple Lines(custom)

```
def combined_plot():
  x = [0,1,2,3,4]
  data = [{
                'x': x,
                'y': list(map(lambda i: i, x)),
                'marker':{
                        'color': 'red',
                        'symbol': 104,
                        'size': "10"},
                'mode':"markers+lines",
                
                'name':'class-1',
                'type':'line',
},{
                'x': x,
                'y': list(map(lambda i: i*2, x)),
                'marker':{
                        'color': 'green',
                        'symbol': 104,
                        'size': "10"},
                'mode':"markers+lines",
                'name':'class-2',
                'type':'line',
  },
{
                'x': x,
                'y': list(map(lambda i: i*3, x)),
                'marker':{
                        'color': 'blue',
                        'symbol': 104,
                        'size': "10"},
                'mode':"markers+lines",
                'name':'class-3',
                'type':'line',
  }
     ,
  {
                'x': x,
                'y': list(map(lambda i: i*4, x)),
                'marker':{
                        'color': 'green',
                        'symbol': 104,
                        'size': "10"},
                'mode':"markers+lines",
                'name':'class-3',
                'type':'line',
  }
  
  ]

  win = 'custom_multiple_lines'
  env = 'main'

  layout= {
                'title':"combined_plot",
                'xaxis':{'title':'x1'},
                'yaxis':{'title':'x2'}
  }
  opts = {}

  vis._send({'data': data, 'win': win, 'eid': env, 'layout': layout, 'opts': opts})
combined_plot()
```
![combined_lines](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/combined_plot_custom.PNG)

## Image
```
img = np.random.rand(3, 512, 256)
vis.images(img, win="IMG", opts=dict(title="IMAGE"))
```

![Img](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/Img.PNG)

## Multiple Images
```
imgs =  np.random.random((4, 3, 128, 128))*255
  vis.images(
        imgs,
        opts=dict(title='Random Images', caption='How random.')
    )
```

![Imgs](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/imgs.PNG)

## Matplotlib
```
import matplotlib.pyplot as plt
plt.plot([1, 7, 2, 4])
plt.title("Line")
plt.ylabel('some numbers')
vis.matplot(plt)
```

![Matplotlib](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/matplotlib.PNG)

## Scatter Plots
```
    # 2D scatterplot with custom intensities (red channel)
    vis.scatter(
        X=np.random.rand(255, 2),
        Y=(np.random.rand(255) + 1.5).astype(int),
        opts=dict(
            markersize=10,
            markercolor=np.random.randint(0, 255, (2, 3,)),
        ),
    )
```

![Scatter Plots](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/2D_scatter_plot.PNG)

## Pie Chart
```
    # pie chart
    X = np.asarray([10, 22, 13, 52])
    vis.pie(
        X=X,
        opts=dict(legend=['A','B','C','D'])
    )
```

![Pie Chart](https://github.com/ninjakx/visualisation-visdom/blob/master/Images/pie_chart.PNG)
