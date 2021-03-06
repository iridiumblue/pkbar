# pkbar
![Test](https://github.com/yueyericardo/pkbar/workflows/Test/badge.svg) [![PyPI version](https://badge.fury.io/py/pkbar.svg)](https://badge.fury.io/py/pkbar)

Keras style progressbar for pytorch (PK Bar)

### 1. show
- `pkbar.Pbar` (progress bar)
```
loading and processing dataset
10/10  [==============================] - 1.0s
```

- `pkbar.Kbar` (keras bar)
```
Epoch: 1/3
100/100 [========] - 10s 102ms/step - loss: 3.7782 - rmse: 1.1650 - val_loss: 0.1823 - val_rmse: 0.4269
Epoch: 2/3
100/100 [========] - 10s 101ms/step - loss: 0.1819 - rmse: 0.4265 - val_loss: 0.1816 - val_rmse: 0.4261
Epoch: 3/3
100/100 [========] - 10s 101ms/step - loss: 0.1813 - rmse: 0.4258 - val_loss: 0.1810 - val_rmse: 0.4254
```

### 2. Install 
```
pip install pkbar
```

### 3. Usage

- `pkbar.Pbar` (progress bar)
```python
import pkbar
import time

pbar = pkbar.Pbar(name='loading and processing dataset', target=10)

for i in range(10):
    time.sleep(0.1)
    pbar.update(i)
```
```
loading and processing dataset
10/10  [==============================] - 1.0s
```

- `pkbar.Kbar` (keras bar) [for a concreate example](https://github.com/yueyericardo/pkbar/blob/master/tests/test.py#L16)
```python
import pkbar
import torch

# training loop
train_per_epoch = num_of_batches_per_epoch

for epoch in range(num_epochs):

    print('Epoch: %d/%d' % (epoch + 1, num_epochs))
    kbar = pkbar.Kbar(target=train_per_epoch, width=8)
    
    # training
    for i in range(train_per_epoch):
        outputs = model(inputs)
        train_loss = criterion(outputs, targets)
        train_rmse = torch.sqrt(train_loss)
        optimizer.zero_grad()
        train_loss.backward()
        optimizer.step()

        kbar.update(i, values=[("loss", train_loss), ("rmse", train_rmse)])

    # validation
    outputs = model(inputs)
    val_loss = criterion(outputs, targets)
    val_rmse = torch.sqrt(val_loss)

    kbar.add(1, values=[("loss", train_loss), ("rmse", train_rmse),
                        ("val_loss", val_loss), ("val_rmse", val_rmse)])
```
```
Epoch: 1/3
100/100 [========] - 10s 102ms/step - loss: 3.7782 - rmse: 1.1650 - val_loss: 0.1823 - val_rmse: 0.4269
Epoch: 2/3
100/100 [========] - 10s 101ms/step - loss: 0.1819 - rmse: 0.4265 - val_loss: 0.1816 - val_rmse: 0.4261
Epoch: 3/3
100/100 [========] - 10s 101ms/step - loss: 0.1813 - rmse: 0.4258 - val_loss: 0.1810 - val_rmse: 0.4254
```

### 4. Acknowledge
Keras progbar's code from [`tf.keras.utils.Progbar`](https://github.com/tensorflow/tensorflow/blob/r1.14/tensorflow/python/keras/utils/generic_utils.py#L313)
