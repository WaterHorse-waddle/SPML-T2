# SPML-T2
# The ResNet18 CNN architecture#
1. Why residual connections help optimization
   
   In residual connections, we introduce 'skip' connections or short-cut connections. These are simple, no operation connections across layers, that directly output the input value x (identty func). So, instead of re-evaluating features in every block, only the difference between expected output and input is added. If the expected output to be calculated is F(x), the model calculates H(x)=F(x)+x. Or essentially, F(x)=H(x)-x.
   
2.  How skip connections improve gradient flow
   I've already stated that we're computing the output as the sum of our main transformation and a short-cut identity. During backpropagation, this addition means the gradient flowing backward through the block splits into two paths one through the learned transformation F(x), and one directly through the identity (or projection) shortcut. The shortcut path provides a route for gradients to reach earlier layers without being repeatedly multiplied by the weights and activation derivatives of every intermediate layer, which is what causes gradients to vanish in very deep plain networks. Even if the gradient through the main path becomes very small, the shortcut path keeps it at least 1. In the research paper that i read on ResNet, the 152 layer residual network was still less complex than plain cnn.

3. The effect of network depth on performance
In my model (4 stages and 2 blocks/stage),  i reached 93.30% validation accuracy on CIFAR10 after 100 epochs and training accuracy of 99.95%. This level of performance at this depth supports the core argument of the ResNet paper: on ImageNet, the researchers found that with residual learning, their deeper 34-layer resnet outperformed the 18-layer resnet by 2.8%, exhibiting lower training error and better generalization to validation data. This was opposite the result they observed for plain cnn networks of the same depths, where the deeper plain network performed worse. This shows depth itself is not the problem andis only beneficial once the optimization difficulty introduced by stacking many layers is solved trhough residual connections.

4. Challenges encountered during implementation
  The most time taking and difficult challenge imo was correctly handling dimension mismatches at stage transitions. Each stage's first block changes both the number of channels and the the spatial resolution (stride=2), which means the shortcut path can no longer be a simple identity. it required a 1x1 convolution projection to match the output shape before the addition can happen. So i had to understand this logic and make a function so that each block tracks the channel no.
Another was that earlier, i had only run 10 epochs. This was producing an uderfit model with training acc of 38.88% and test accuracy of 41.06%. So, i had to increase the epochs to 100. Papers suggest more, but due to network and other issues, i had to limit to 100.
