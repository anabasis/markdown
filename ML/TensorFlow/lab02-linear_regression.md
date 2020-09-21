# LAB02

## Hypothesis and cost function

$$H(\ x\ )\ =\ W_x\ +\ b$$
$$cost(\ W\ ,\ b\ )\ =\ \frac{1}{m}\ \sum_{i=1}^{m}\ (\ H(\ x^{(i)})\ -\ y^{(i)}\ )\ ^2$$

```python
x_train = [1,2,3]
y_train = [1,2,3]

W = tf.Variable(tf.random_normal([1]), name = 'weight')
b = tf.Variable(tf.random_normal([1]), name = 'bias')

# Our hypothesis XW+B
hypothesis = x_train * W + b

# cost/loss function
cost = tf.reduce_mean(tf.square(hypothesis - y_train))
```

tf.Variable : <https://www.tensorflow.org/api_docs/python/tf/Variable>
tf.random_normal : <https://www.tensorflow.org/api_docs/python/tf/random_normal>
tf.reduce_mean : <https://www.tensorflow.org/api_docs/python/tf/reduce_mean>
tf.square : <https://www.tensorflow.org/api_docs/python/tf/square>

```python
# Minimize *************
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)

# Launch the graph in a session
sess = tf.Session()
# Initializes global variables in the graph.
sess.run(tf.global_variables_initializer())

#Fit the Line
for step in range(2001):
    sess.run(train)
    if step % 20 == 0:
        print(step, sess.run(cost), sess.run(W), sess.run(b))
```

tf.train.GradientDescentOptimizer : <https://www.tensorflow.org/api_docs/python/tf/train/GradientDescentOptimizer>
tf.global_variables_initializer : <https://www.tensorflow.org/api_docs/python/tf/global_variables_initializer>

tf.train.GradientDescentOptimizer.minimize

```json
minimize(
    loss,
    global_step=None,
    var_list=None,
    gate_gradients=GATE_OP,
    aggregation_method=None,
    colocate_gradients_with_ops=False,
    name=None,
    grad_loss=None
)
```