# visualisation-visdom
Making plots using visdom


## Heatmaps
`
vis = visdom.Visdom(port='6002')
import torchnet as tnt
import torch 
confusion_matrix = tnt.meter.ConfusionMeter(3) # 3 classes 
y_actu = np.array([2, 2, 1, 2, 0, 2, 1])
y_pred = np.array([2, 2, 1, 2, 0, 2, 1])
confusion_matrix.add((torch.from_numpy(y_actu)),(torch.from_numpy(y_pred)))
print(confusion_matrix.value)

vis.heatmap(
        X=confusion_matrix.value(),
        opts=dict(
            columnnames=['a', 'b', 'c'],
            rownames=['a', 'b', 'c'],
            colormap='Electric',
        )
    )
`
