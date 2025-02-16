矩阵求导术

常用公式
$$
\begin{align}
\nabla_{\mathbf{x}}(\mathbf{A}\mathbf{x}) &= \mathbf{A}^T \\
\nabla_{\mathbf{x}}(\mathbf{x}^T\mathbf{A}) &= \mathbf{A} \\
\nabla_{\mathbf{x}}(\mathbf{x}^T\mathbf{A}\mathbf{x}) &= (\mathbf{A} + \mathbf{A}^T)\mathbf{x} \\
\nabla_{\mathbf{x}}||\mathbf{x}||^2 &= \nabla_{\mathbf{x}}(\mathbf{x}^T\mathbf{x}) = 2\mathbf{x}
\end{align}

$$
Examples
$$
\frac{\partial}{\partial \mathbf{w}} ||\mathbf{y} - \mathbf{X}\mathbf{w}||^2 = 2\mathbf{X}^T (\mathbf{X}\mathbf{w} - \mathbf{y}) = 0 \text{ and hence } \mathbf{X}^T\mathbf{y} = \mathbf{X}^T\mathbf{X}\mathbf{w}
$$