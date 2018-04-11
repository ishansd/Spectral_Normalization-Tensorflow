# Spectral_Normalization-Tensorflow
 Simple Tensorflow Implementation of "Spectral Normalization for Generative Adversarial Networks" (ICLR 2018)
 
 ## Summary
 ![sn](./assests/sn.png)
 
 ```python
def spectral_normed_weight(w, iteration=1):
    w_shape = w.shape.as_list()
    w = tf.reshape(w, [-1, w_shape[-1]])

    u_hat, v_hat = None, None

    u = tf.get_variable("u", [1, w_shape[-1]], initializer=tf.truncated_normal_initializer(), trainable=False)
    u_iter = u

    for i in range(iteration):
    
        v_ = tf.matmul(u_iter, tf.transpose(w))
        v_hat = _l2normalize(v_)

        u_ = tf.matmul(v_hat, w)
        u_hat = _l2normalize(u_)
        u_iter = u_hat

    sigma = tf.matmul(tf.matmul(v_hat, w), tf.transpose(u_hat))
    w_norm = w / sigma

    with tf.control_dependencies([u.assign(u_hat)]):
        w_norm = tf.reshape(w_norm, w_shape)

    return w_norm
 ```
 ## Author
 Junho Kim
