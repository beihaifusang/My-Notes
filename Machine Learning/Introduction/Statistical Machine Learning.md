# Machine Learning 

### 1. Supervised Learning

- Feature normalization: $\forall x_{ij} \in X, x_{ij}=\large\frac{x_{ij}-\mu_j}{\sigma_j^2}$, $\small X:[instance][feature]$, without $\small[1...1]^T$ in 1st column
  - $X=[x_1,x_2,...,x_m]$, m instances in total
- Regularization: add penalty for $\theta$ being large into cost function
  - $\displaystyle J(\theta)= \space... + \frac{\lambda}{2m}\sum^n_{j=1}\theta_j^2$ , **$\theta_0$ shouldn't be penalized** 
- Matlab/Octave tips:
  - Unrolling parameters: deltaVector = [ D1(:) ; D2(:) ] // *matrix D1,D2 => column vector* 
  - Reshape: D1 = reshape (deltaVector(1, m\*n), m, n) // *column vector => m\*n matrix* 

#### Linear Regression

- Assumption: $\text{$y=h^\star_\theta(x)+\epsilon$} \text{, where $h^\star_\theta(x)$ is the theoretical (perfect) hypothesis, $\epsilon$ is gaussian noise}$ 

- General Hypothesis Function: $h_\theta(x)=\phi (x)\theta$, where $\phi (x)$ is basis function

- Log Likelihood leads to least square cost function $J(\theta)$ 
  - $P(y|x,\theta, \beta) = \mathcal{N}(x|h_\theta(x), \beta^{-1})$ - likelihood
    - $\text{based on assumption: $y=h^\star_\theta(x)+\epsilon$} \\ \text{where $h^\star_\theta(x)$ is the theoretical (perfect) hypothesis, $\epsilon$ is gaussian noise}$ 
  - $\displaystyle P(\boldsymbol y|X,\theta,\beta) = \prod_{i=1}^m \mathcal{N}(x^i|h_\theta(x^i), \beta^{-1})$ 
  - $\displaystyle \ln P(\boldsymbol y|X,\theta,\beta) = \frac N 2\ln\beta - \frac N 2 \ln(2\pi) - \beta \frac 1 2\sum_{i=1}^m (h_\theta(x^i) - y^i)^2$ 

- Log Posterior leads to regularization
  - Maximizing the likelihood function => (always) excessively complex models & over-fitting
  - Regularization term comes from the $\text{Prior}$: 
    - $\text{assume Prior } p(\theta) =  \mathcal{N}(\theta|0,\alpha^{-1}I) \small \text{, so that Posterior & Prior are of the same distribution} \\ \text{to maximize log Posterior}: \\ \displaystyle \Rightarrow \ln p(\theta|X) \propto -\frac \beta 2 \sum_{i=1}^n (y^i - h_\theta(x))^2 - \frac \alpha 2 \theta^T\theta + const $ 
    - If $\alpha \to 0 \text { (Prior is most useless)}$, maximize $\text{Posterior}$ is equivalent to maximizing likelihood 
    - Maximize $\text{Posterior} \Leftrightarrow$ Minimize $\text{cost function with regularization}$, where $λ = α/β$ 

- Predictive Distribution: $p(y|x,X,Y)$ 
  - $\displaystyle p(y|x,X,Y) = \int p(y,\theta|x,X,Y)d\theta = \int p(y|\theta,x,X,Y)p(\theta|x,X,Y)d\theta$ 
  - $p(y|\theta,x,X,Y)=p(y|\theta,x) = \mathcal{N}(y|h(x,\theta), \beta^{-1}) \\ \small  \text{based on assumption: } y = y(x,\theta)+\epsilon \text{, where $\epsilon$ is Guassian noise} \\ p(\theta|x,X,Y)=p(\theta|X,Y) = \text{posterior}$ 
  - $ \displaystyle \Rightarrow p(y|x,X,Y)=\int p(y|\theta,x) p(\theta|X,Y)d\theta$   

- $Expected \space Lost  = (bias)^2 + variance + noise$ 

  - Notation:

    - $t=y(x,w)+\epsilon,\text{ where } \epsilon \text{ is Gaussian noise}$ 
    - $\hat y$ is prediction function to approximate $y=y(x,w)$ 

  - Procedure:

    - $\mathbb E[(t-\hat y)^2] = \mathbb E[t^2-2t\hat y+\hat y^2] \\ = \mathbb E[t^2] +\mathbb E[\hat y^2] -\mathbb E[2t\hat y] \\ = \text{Var}[t]+\mathbb E[t]^2 + \text{Var}[\hat y]+\mathbb E [\hat y]^2 - 2y\mathbb E[\hat y] \\ = \text{Var}[t] + \text{Var}[\hat y] + (y^2 -2y\mathbb E[\hat y] +\mathbb E[\hat y]^2) \\ =  \text{Var}[t] + \text{Var}[\hat y] + (y-\mathbb E[\hat y])^2 \\ =  \text{Var}[t] + \text{Var}[\hat y] + \mathbb E[t-\hat y]^2 \\ = \sigma^2+\text{Var}[\hat y] + \text{Bias[$\hat y$]}^2$ 

      $\text{where } \sigma^2 = \text{Var}[\epsilon] \text{ is the noise}$ 

      (formula used: $\text{Var}[x] = \mathbb E[x^2] - \mathbb E[x]^2 \Leftrightarrow \mathbb E[x^2] = \text{Var}[x]+ \mathbb E[x]^2$) 

- Matrix inverse can be evil & avoid inverse operation: 

  $A = U\Lambda U^T  \text{, where $\Lambda$ is diagonal matirx} \\ => A^{-1} = U\Lambda^{-1}U^T \\ \text {but number on the diagonal line of $\Lambda$ can be small => maybe 0 depending on accuracy of computer}$ 

1. Bayesian Regression:

   - Assumption:

     - $t=y(x,w)+\epsilon,\text{ where } \epsilon \text{ is Gaussian noise}; y(x,w)\text{ approximated by }\phi(x)w$ 

   - Bayesian view:

     - Gaussian $\text{Prior}: p(w) = \mathcal N(w|m_0,S_0)$ 

       ​	$\small \text{Reason: to be conjugate}$ 

     - $\text{Likelihood}: \displaystyle p(\boldsymbol t|w) = \prod_{n=1}^N\mathcal N(t_n|w^T\phi(x_n),\beta^{-1}) = \mathcal N(\boldsymbol t|\Phi w,\beta^{-1}I)$ 

     - $\Rightarrow \text{Posterior}: p(w|\boldsymbol t) = \mathcal N(w|m_N,S_N)$ 

       ​	 $\text{where } m_N = S_N(S_0^{-1}m_0+\beta \Phi^T\boldsymbol t),\space S_N^{-1} = S_0^{-1}+\beta \Phi^T\Phi $ 

   - Maximum Likelihood:

     - $\text{Likelihood}: \displaystyle p(\boldsymbol t|w) = \prod_{n=1}^N\mathcal N(t_n|\phi(x_n)w,\beta^{-1})$ 

     - $\displaystyle \ln \text{Likelihood} = \sum_{n=1}^N [-\ln \frac {\beta} {\sqrt {2\pi}} - \frac \beta 2 (t_n-\phi(x)w)^2]$ 

     - $\displaystyle \frac {\partial} {\partial w} \ln \text{Likelihood} = \beta \Phi^T(\boldsymbol t-\Phi w)$ 

       let $\frac {\partial} {\partial w} \ln \text{Likelihood}=0$ 

       $\displaystyle \Rightarrow w_{ML} = (\Phi^T\Phi)^{-1} \Phi^T\boldsymbol t $ 

     - $\displaystyle \frac {\partial} {\partial \beta} \ln \text{Likelihood} = -N\beta^{\frac 1 2} + \beta^{\frac 3 2}(\boldsymbol t-\Phi w)^T(\boldsymbol t-\Phi w) $  

       let $\frac {\partial} {\partial \beta} \ln \text{Likelihood}=0$ 

       $\displaystyle \Rightarrow \beta^{-1}=\frac 1 N (\boldsymbol t-\Phi w)^T(\boldsymbol t-\Phi w)$ 

       ​	$\text{Note: solve $w=w_{ML}$ first}$ 

   - Maximum Posterior:

     - $\text{Posterior}=p(w|\boldsymbol t), \text{Prior}=p(w), \text{Likelihood} = p(\boldsymbol t|w), \text{Normalization}=p(t) \\ \displaystyle \Rightarrow \text{Posterior}=\frac {\text{Likelihood*Prior}} {\text{Normalization}}$ 

       $\Rightarrow \text{Posterior} \propto \text{Likelihood*Prior} $  

     - $\text{assume Prior } \displaystyle p(w) =  \mathcal{N}(w|m_0,S_0), \\ \small \text{so that Prior & Likelihood are conjugate} \Rightarrow \text{Gaussian Posterior}$ 

     - $\text{Likelihood } \displaystyle p(\boldsymbol t|w) = \prod_{n=1}^N\mathcal N(t_n|\phi(x_n)w,\beta^{-1}) =\mathcal N(\boldsymbol t|\Phi w,\beta^{-1}I )$ 

     - $\Rightarrow \text{Posterior } \displaystyle p(w|\boldsymbol t) = \mathcal N(w|m_N,S_N),\\ \text{where } m_N=S_N(S_0^{-1}m_0+\beta\Phi^T \boldsymbol t),\space S_N^{-1}=S_0^{-1} + \beta \Phi^T\Phi$ 

       $\Rightarrow w_{MAP} = \text{mean of the Gaussian} = m_N$ 

       ​	$\text{Note: can also get } w_{MAP} \text{ from taking gradient}$ 

     - Simple Prior:

       $\text{Prior } p(w)=\mathcal N(w|0,\alpha^{-1}I)$ 

       $\Rightarrow \text{Posterior } \displaystyle p(w|\boldsymbol t) = \mathcal N(w|m_N,S_N),\\ \text{where } \displaystyle m_N=\beta(\alpha I + \beta \Phi^T\Phi)\Phi^T \boldsymbol t,\space S_N^{-1}=\alpha I + \beta \Phi^T\Phi$ 

       $ w_{MAP} \rightarrow w_{ML}, \text{ when }\alpha \rightarrow 0 \text{ (most useless Prior)}$ 

     - Maximize $\text{Posterior} \Leftrightarrow$ Minimize $\text{cost function with regularization}$: 

       ​	$ \text{Simple Prior} \Rightarrow \displaystyle \ln p(w|\boldsymbol t) = -\frac \beta 2 (\boldsymbol t-\Phi w)^T (\boldsymbol t-\Phi w)-\frac \alpha 2 w^Tw+const$ 

       - If $\alpha \to 0 \text { (Prior is most useless)}$, maximize $\text{Posterior}$ is equivalent to maximizing likelihood 
       - Maximize $\text{Posterior}$ equal to minimize sum-of-squares error function with the addition of a quadratic regularization term with $λ = α/β$ 
       - Regularization term comes from the $\text{Prior}$ 

   - Predictive Distribution:

     - Assume: $\text{Prior}: p(x|\alpha) = \mathcal N(x|0,\alpha^{-1}I),\space \small\text{ (}m_0=0,S_0=\alpha^{-1}I \text{)}$ 

     - $\displaystyle p(t|x,X,\boldsymbol t) = \int p(t|w,x)p(w|X,\boldsymbol t)dw$ 

     - $\Rightarrow p(t|x,X,\boldsymbol t) = \mathcal N(t|m_N^T\phi(x),\sigma^2_N(x))$ 

       ​	$\text{where } \sigma_N^2(x)=\frac 1 \beta + \phi(x)^TS_N\phi(x);\space m_N, S_N \text{ from Posterior} \small (m_N=w_{MAP})$ 

   - Sequential data:

     - $\text{Posterior}$ from previous data $\Leftrightarrow$ the $\text{Prior}$ for the arriving data
     - Sequential data and data in one go is equivalent when finfding the Porsterior

2. Gradient descent

   - Hypothesis function: $h_\theta(x)=x\theta$, $\small\theta = [\theta_0, \theta_1, ..., \theta_n]^T$, $\small x=[x_0, x_1,..., x_n], x_0=1$
     - $x$ is one instance
   - Cost function: $\displaystyle J(\theta)=\frac{1}{2m}\sum_{i=1}^m(h_\theta(x^i)-y^i)^2+\frac{\lambda}{2m}\sum^n_{j=1}\theta_j^2$ 
   - Update rule: $\forall \theta_j \in \theta, \theta_j := \theta_j-\alpha\large\frac{\partial J(\theta)}{\partial\theta_j}$, $\displaystyle \small\frac{\partial J(\theta)}{\partial\theta_j}=\frac{1}{m}\sum^m_{i=1}[(h_\theta(x^i)-y^i)x_j^i] + \frac\lambda m\theta_j$ 
     - $\displaystyle \frac {d}{d\theta}J(\theta) = \frac 1m ((X\theta-y)^TX)^T + \frac \lambda m [0,\theta_1,...,\theta_m]^T$ ($\theta_0$ shouldn't be penalized)
     - simultaneously for all $\theta_j \in \theta$ 
   - Normal equation (mathmetical solution)
     - $\theta = (X^TX)^{-1}X^Ty$  

#### Logistic Regression (Classification)

- Decision Theory:
  - classes $C_1,...,C_K$, decision regions $\mathcal {R}_1,...,\mathcal{R}_K$ 
    - Minimze $\displaystyle p(mistake) = \sum_{k=1}^K (\int_{\mathcal {R}_k} \sum_{i \neq k} p(x,C_i)dx)$ $\small \text{(can have weight on each misclassfication though)}$  
    - Maximize $\displaystyle p(correct) = \sum_{k=1}^K \int _{\mathcal {R}_k}p(x,C_k)dx$ 

- Models for Decision Problems:

  - Find a discriminant function
  - Discriminative Models: $\text{less powerful, yet less parameter => easier to learn}$ 
    - Infer **posterior** $p(C_k|x)$, $\small \text{$C_k$: $x \in C_k$, $x$ is examples in trainning set}$ 
    - Use decision theory to assign a new $x$ 
  - Generative Models: $\text{more powerful, but computationally expensive}$ 
    - Infer conditional probabilities $p(x|C_k)$ 
    - Infer prior $p(C_k)$ 
    - Find **either** **posterior** $p(C_k|x)$, **or** **joint distribution** $p(x, C_k)$ (using Bayes' theorem)
    - Use decision theory to assign a new $x$ 
    - **=> Able to create synthetic data using $p(x)$** 

- Naive Bayes on Discrete Features:

  - Assumption:

    - Discrete Features: data point $x\in\{0,1\}^D$ 

    - Naive Bayes: all features conditioned on class $C_k$ are independent with each other

      $\displaystyle \Rightarrow p(x|C_k)=\prod_{i=1}^D \mu_{ki}^{x_i} \space (1-\mu_{ki})^{1-x_i}$ 

1. Linear Discriminant (Least Squares Approach)

   - Prediction:

     - $y(x)=xw+w_0 \text{, with bias $=w_0$, where } w = [w_1,...,w_n]^T, x=[x_1,...,x_n]$ 
     - $y(x)>0$: positive confidence to assign $x$ to current class 
     - $-w_0$ called threshold sometimes

   - Decision Boundary $y(x)=w^Tx+w_0=0$: 

     - $w$ is orthogonal to vectors on the boundary:

       ​	$\text{assume } x_1,x_2 \text{ on the boundary} \\ \Rightarrow 0=y(x_1) - y_(x_2)=(x_1-x_2)w$ 

     - Distance from origin to boundary is $-\frac {w_0} {\|w\|}$: 

       ​	$\text{assume distance is } k \\ \Rightarrow k \frac {w}{\|w\|} \text{on boundary, thus } k \frac {w}{\|w\|} w + w_0=0 \\ \Rightarrow k=-\frac {w_0}{\|w\|}$ 

     - $y(x)$ is a signed measure of distance from point $x$ to boundary:

       ​	$\text{assume distance is }r \\ \Rightarrow y(x)=\overbrace{(x_\perp+r \frac {w}{\|w\|})}^x w + w_0 = \overbrace{x_\perp w+w_0}^0 + r\|w\| = r\|w\| \\ \Rightarrow r = \frac{y(x)} {\|w\|}$ 

   - Multi-class $\small\text{(k-classes)}$:

     - prediction: $x$ is of class ${\mathcal C}_k$ if $\forall j \neq k, y_k(x) > y_j(x)$ 

       $\Rightarrow y(x)=xW \text{, where } W=[w_1,...,w_k],\forall x_i \in X,x_{i0}=1 {\small\text{ (bias)}}, y(x) \text{ is 1-of-k coding} $ 

     - sum-of-squares error: $E_D(W)=\frac 1 2 \text{tr}\{(XW-T)(XW-T)^T\}$ 

       $\Rightarrow \text{optimal } W = (X^TX)^{-1}X^TT$ 

       ​	$\small \text{note that } tr\{ AB \} = A^TB^T$ 

2. Fisher’s Linear Discriminant

   - Basic idea:
     - Take linear classification $y = w^Tx$ as dimensionality reduction (projection onto 1-D)
     - => find a projection (denoted by vector $w$) which maximally preserves the class separation
     - => if $y>-w_0$ then class $C_1$, otherwise $C_2$ 

   - Goal:

     - Distance within one class is small
     - Distance between classes is large

   - Mean & Variance of Projected Data:

     - Mean: $\displaystyle\widetilde m_k = w^Tm_k, \text{ where } m_k = \frac 1 {N_k}\sum_{x\in C_k} x$ 
     - Variance: $\displaystyle \widetilde s_k=\sum_{x\in C_k}(w^Tx-w^Tm_k)^2 =   w^T[\sum_{x\in C_k}(x-m_k)(x-m_k)^T]w$ 

   - 2-Classes to 1-D line:

     - Maximize Fisher criterion: $\displaystyle J(w) = \frac {|\widetilde m_1 - \widetilde m_2|^2} {{\widetilde s_1}^2+{\widetilde s_2}^2}$ 

     - Between-class covariance:  $\displaystyle S_B = (m_1-m_2)(m_1-m_2)^T$ 

     - Within-class covariance: $\displaystyle S_k=\sum_{n\in C_k}(x_n-m_k)(x_n-m_k)^T$ 

       $\displaystyle\Rightarrow J(w) = \frac {w^TS_B w} {w^T S_W w} $ 

     - Lagrangian: $L(w,\lambda) = w^TS_B w + \lambda(1- w^T S_W w)$ 

       ​	$\small \text{fix $w^T S_W w$ to 1 to avoid infinite solution (any multiple of a solution is a solution)}$

       $\Rightarrow \displaystyle \frac \partial {\partial w}L = 2S_Bw-2\lambda S_Ww=0 \\ \Rightarrow S_Bw=\lambda S_Ww \\ \Rightarrow ({S_W}^{-1}S_B)w=\lambda w \\ \text{To maximize $J(w)$, $w$ is the largest eigenvector of ${S_W}^{-1}S_B$ if $S_W$ invertible} $ 

   - K-classes to a d-D subspace: $\small N_k \text{ is num in class k, $N$ is the total example num}$ 

     - Between-class covariance:  $\displaystyle S_B = \sum_{k=1}^K N_k(m_k-m)(m_k-m)^T, \text{where } m=\frac 1 N \sum_{n=1}^N x_n  \\ \small \text{reduce to } (m_1-m_2)(m_1-m_2)^T  \text{ when K=2 (constant ignored)}$ 

     - Within-class covariance: $\displaystyle S_W = \sum_{k=1}^KS_k, \small \text{where } S_k=\sum_{n\in C_k}(x_n-m_k)(x_n-m_k)^T, m_k = \frac 1 {N_k} \sum_{n \in C_k}x_n$ 

     - Maximize Fisher criterion: $\displaystyle J(w) = \frac {tr\{W^TS_B W\}} {tr\{W^T S_W W\}}$ 

     - Lagrangian: 

       $\text{Solve for each } w_i\in W \Rightarrow ({S_W}^{-1}S_B)w_i=\lambda_i w_i \\ \Rightarrow W \text{ conosists of the largest d eigenvectors} \\ \small {S_W}^{-1}S_B \text{ is not guaranteed to be symmetric } \Rightarrow W \text{ might not be orthogonal} \\ \small\text{Need to minimize the whole covariance matrix ($J(w)$ as a matrix) $\Rightarrow$ not choosing same eigenvectors twice} $  

     - Maximum Possible Projection Directions = $K-1$: 

       ​	$r({S_W}^{-1}S_B) \le \min(r({S_W}^{-1}),r(S_B)) \le r(S_B) \\ r(S_B) \le \displaystyle \sum_K r((m_k-m)(m_k-m)^T) = K \small \text{, as } r(m_k-m)=1 \\ \displaystyle \sum_Km_k = m \Rightarrow r(m_1-m,...,m_K-m)=K-1 \\ \Rightarrow r(S_B)\le K-1 \\ \Rightarrow r({S_W}^{-1}S_B) \le K-1$ 

3. Perceptron Algorithm

   - Generalised linear model $y = f(w^T\phi(x))$, where $\phi(x)$ is basis function; $ \phi_0(x) = 1$ 

   - Nonlinear activation funtion: $f(a) = \begin{cases} 1,& a \ge0 \\-1,& a<0  \end{cases}$ 

   - Target coding: $t = \begin{cases} 1,&\text{if } C_1 \\ -1, &\text{if } C_2 \end{cases}$ 

   - Cost function: 

     - All correctly classified patterns: $w^T\phi(x_n)t_n>0$ 

     - Add the errors for all misclassified patterns (denoted as set $\mathcal{M}$):

       $\Rightarrow \displaystyle E_P(w)=-\sum_{n\in \mathcal{M}} w^T \phi(x_n)t_n$ 

   - Algorithm: (Aim: minimize total num of misclassified patterns)

     - loop 

       ​	choose a traning pair $(x_n, t_n)$ 

       ​	update the weight vector $w$: $w  = w - \eta \nabla E_p(w) = w+\phi_nt_n$ 

       ​		$\text{ where $\eta$=1 because $y=f(\cdot)$ does not depend on $\|w\|$}$ 

   - Perceptron Convergence Theorem:

     - If the training set is linearly separable, the perceptron algorithm is guaranteed
       to find a solution in a finite number of steps

       $\small \text{(Also is the algorithm to find whether the set is linearly separable => Halting Problem)}$ 

4. Maximum Likelihood

   - Assumption: 
     - $p(x|C_k) \sim\mathcal{N}(\mu_k,\Sigma), \text{and all } p(x|C_k) \text{ share the same } \Sigma$ 
     - $p(C_1) = \pi,\space p(C_2)=1-\pi \text{, $\pi$ unknown}$ 
   - Likelihood of whole data set $\boldsymbol{X,t}$, $N$ is the num of data
     - $\displaystyle p(\boldsymbol{X,t}|\pi, \mu_1, \mu_2, \Sigma) = \prod_{n=1}^N[\pi\mathcal{N}(x_n|\mu_1,\Sigma)]^{t_n} \space [(1-\pi)\mathcal{N}(x_n|\mu_2, \Sigma)^{1-t_n}]$ $\space\small\rightarrow\text{when info of label $t$ lost: mixture of Gaussian}$  
     - $\displaystyle \ln (\text{Likelihood}) = \sum_{n=1}^N[t_n(\ln\pi+\ln\mathcal{N}(x_n|\mu_1,\Sigma)) + (1-t_n)(\ln(1-\pi)+\ln\mathcal{N}(x_n|\mu_2, \Sigma))]$ 
   - Parameters when maximum reached:
     - $\displaystyle \pi = \frac {N_1}{N_1+N_2}$, $N_1$ is the num of class $C_1$ 
     - $\displaystyle \mu_1 = \frac 1 {N_1} \sum_{n=1}^Nt_n x_n, \space \mu_2=\frac 1 {N_2}\sum_{n=1}^N(1-t_n)x_n,$ (mean of each class)
     - $\displaystyle \Sigma = \frac {N_1}{N}S_1 + \frac {N_2}NS_2, \text {where } S_k = \frac 1 {N_k}\sum_{n \in C_k}(x_n-\mu_k)(x_n-\mu_k)^T $ 

5. Logistic Regression 

   - Sigmoid function: $\displaystyle \sigma(a) = \frac 1 {1+e^{-a}}$ 

   - $p(x|C_k) \sim \mathcal{N} \implies p(C_k|x)=\sigma(w^Tx+w_0) \small \text{ (2-classes)}$  (Generative model)

     - Assumption:

        $p(x|C_k) = \mathcal{N}(\mu_k, \Sigma)$ (can also be a number of other distributions)

        $\forall k, p(x|C_k) \text{ shares the same } \Sigma$ 

     - $\displaystyle p(C_1|x)=\frac{p(x|C_1)p(C_1)}{p(x|C_1)p(C_1)+p(x|C_2)p(C_2)} = \sigma (a), \\ \begin{align*} \text{where } a &=\ln \frac {p(x,C_1)}{p(x,C_2)} \\ &= \ln \frac {p(x|C_1)p(C_1)}{p(x|C_2)p(C_2)} \\ &= \dots \text{(assumption applied)} \\ &= \ln \frac {\text{exp}(\mu_1^T\Sigma^{-1}x - \frac 12 \mu_1^T\Sigma^{-1}\mu_1)}{\text{exp}(\mu_2^T\Sigma^{-1}x - \frac 12 \mu_2^T\Sigma^{-1}\mu_2)} + \ln \frac {p(C_1)}{p(C_2)}  \\ \implies & a = w^Tx+w_0  \text{ where, } \\ &\space w = \Sigma^{-1}(\mu_1-\mu_2)  \\ & \space w_0 = -\frac 12\mu_1^T\Sigma^{-1}\mu_1 + \frac 12 \mu_2^T\Sigma^{-1}\mu_2 + \ln \frac {p(C_1)}{p(C_2)} \end{align*}$ 

     - $\displaystyle \implies p(C_1|x) = \sigma(w^Tx+w_0)$ 

       $\Rightarrow$ Find parameters in Gaussian model using Maximal Likelihood Sulotion

       ​	as: $p(C_1|x)\propto p(x|C_1)p(C_1)=p(x,C_1)$ 

     - Generalize to $\text{K-classes}​$:

       $\displaystyle a_k(x) = \ln [p(x|C_k)p(C_k)], \space p(C_k|x) = \frac{\text{exp}(a_k)}{\sum_i \text{exp}(a_i)}$ 

       $\Rightarrow \displaystyle a_k(x) = w_k^Tx+w_{k0}, \text{where } w_k=\Sigma^{-1}\mu_k; \space w_{k0} = -\frac 1 2 \mu_k^T\Sigma^{-1}\mu_k + p(C_k)$ 

   - $\text{Assume directly } p(C_k|x)=\sigma(w^Tx+w_0) \small \text{ (2-classes)}$ (Discriminative model)

     - Assume directly: $p(C_1|w,x) = \sigma(w^Tx), \space x_0 = 1$ 

       ​	$\Rightarrow$ less parameters to fit (compared to Gaussian)

     - Likelihood function: 

       $\displaystyle p(\boldsymbol{t}|w,X) = \prod_{n=1}^Np_n^{t_n}(1-p_n)^{1-t_n}, \text{ where, } p_n=p(C_1|x_n), \space t_n \text{ is the class of } x_n$  

       Define error function :

       ​	$\displaystyle E(w) = -\ln(Likelihood) = - \sum_{n=1}^N [t_n\ln p_n + (1-t_n)\ln(1-p_n)]$ 

       $\displaystyle \Rightarrow \nabla E(w)=\sum_{n=1}^N(p_n-t_n)x_n$ 

     - Find Posterior $p(w|\boldsymbol{t})$: 

       $\text{Likelihood}$ is product of sigmoid

       Conjugate $\text{Prior}$ for "sigmoid distribution" is unknown

       $\Rightarrow$ Assume $\text{Prior } p(w) = \mathcal{N}(w|m_0, S_0)$ 

       $\Rightarrow \displaystyle \ln p(w|\boldsymbol{t}) \propto -\frac 1 2 (w-m_0)^TS_0^{-1}(w-m_0) + \sum_{n=1}^N[t_n\ln p_n + (1-t_n) \ln(1-p_n) ]$ 

       ​	find $w_{MAP}$, calculate $\displaystyle S_N = -\nabla \nabla \ln p(w|\boldsymbol{t}) = S_o^{-1} + \sum_{n=1}^N p_n(1-p_n)\phi_n\phi_n^T$ 

       $\Rightarrow p(w|\boldsymbol{t}) \simeq \mathcal{N}(w|w_{MAP}, S_N), \small \text{via Laplace Approximation}$ 

   - Laplace Approximation: 

     - Fit a guassian to $p(z)$ at its **mode** ( mode of $p(z)$: point where $p'(z)=0$ )

     - Assume $p(z) = \frac 1 Z f(z), \text{with normalization } Z = \int f(z)dz$ 

       Taylor expansion of $\ln f(z)$ at $z_0$: $\ln f(z) \simeq \ln f(z_0)-\frac 1 2A(z-z_0)^2,$ 

       ​	$\text{where } f'(z_0)=0, A=- \frac {d^2}{dz^2}\ln f(z) |_{z=z_0}$ 

       Take its exponentiating: $f(z) \simeq f(z_0)\text{exp{$-\frac A 2 (z-z_0)^2$}}$ 

       $\displaystyle \Rightarrow \text{Laplace Approximation} = (\frac A {2\pi})^{1/2} \text{exp{$-\frac A 2 (z-z_0)^2$}} \text{, where }A=\frac 1 {\sigma^2}$ 

     - Requirement:

       ​	$f(z)$ differentiable to find a critical point

       ​	$f''(z_0) < 0$ to have a maximum & so that $\nabla \nabla\ln f(z_0)=A>0$ as $A=\frac1 {\sigma^2}$ 

     - In Vector Space: approximate $p(z)​$ for $z\in \mathcal{R}^M​$ 

       Assume $p(z)=\frac 1 Z f(z)$

       Taylor expansion: $\ln f(z) \simeq \ln f(z_0) - \frac 1 2 (z-z_0)^TA(z-z_0),$ 

       ​	$\text{Hessian } A = - \nabla \nabla \ln f(z)|_{z=z_0} $ 

       $ \begin{align}  \Rightarrow \text{Laplace approximation}&= \frac {|A|^{1/2}}{(2\pi^{M/2})}\text{exp{$-\frac 1 2 (z-z_0)^TA(z-z_0)$}} \\ &= \mathcal{N}(z|z_0, A^{-1}) \end{align}$ 

   - Gradient descent:

     - Hypothesis function: $h_\theta(x)=\sigma(x\theta)=\large\frac{1}{1+e^{-x\theta}}$ 

     - Cost function: 

       $\displaystyle J(\theta)=\frac{1}{m}\sum^m_{i=1}[-y^i \ln (h_\theta(x^i))-(1-y^i) \ln (1-h_\theta(x^i))]+\frac{\lambda}{2m}\sum^n_{j=1}\theta_j^2$ 

     - Update rule: $\forall \theta_j \in \theta, \theta_j := \theta_j-\alpha\large\frac{\partial J(\theta)}{\partial\theta_j}$, $\displaystyle \small\frac{\partial J(\theta)}{\partial\theta_j}=\frac{1}{m}\sum^m_{i=1}[(h_\theta(x^i)-y^i)x_j^i] + \frac \lambda m\theta_j$ 


### 3. Diagnose Machine Learning

- Size of data set
- value of $\lambda$
- Num of feature
- Degree of polynomial
- Build a quick implmentation

#### Evaluating hypothesis

1. Data set => training set (60%), cross validation set (20%), test set (20%)
   - randomly split

2. Cross Validation:

   - estimation of the generalization error, inaccurate because:
     - finite trainning set
     - finite corss validation set

   - S-fold Cross Validation:

     - devide whole set of data into S sets

     - for $i \in (1,S)$ 

       ​	choose the $i^{th}$ set as cross validation (or test set in some cases)

       ​	rest of the sets are called traning set

       ​	run the procedure (mentioned later) on the traning set

       ​	estimate generalization in CV set 

3. Test set error

   - Linear regression:  $\displaystyle J_{test}(\theta)=\frac1{2m_{test}}\sum^{m_{test}}_{i=1}(h_\theta(x^i_{test})-y^i_{test})^2$ 
   - Logistic regression: $\displaystyle J_{test}(\theta)=-\frac1{m_{test}}\sum^{m_{test}}_{i=1}(y_{test}^i log\space h_\theta (x_{test}^i) + (1-y_{test}^i)log\space h_\theta(x_{test}^i))$ 
   - Misclassfication error: $\displaystyle J_{test}(\theta) = \frac1 {m_{test}}\sum_{i=1}^{m_{test}}err(h_\theta(x_{test}^i), \space y_{test}^i)$ 
     - $\begin{equation} err(h_\theta(x), \space y) = \begin{cases}1 & \text{if $h_\theta(x) \geq 0.5, y=0\\ \text{or $h_\theta(x) < 0.5, y=1$}$} \\ \\ 0 & \text{otherwise} \end{cases} \end{equation}$ 

4. Choosing procedure:
   - Minimize traning error $J_{train}(\theta)$ 
   - Select a model with lowest $J_{cv}(\theta)$ 
   - Estimate generalization error as $J_{test}(\theta)$ 


#### Bias / Variance

1. Problems:
   - High bias: underfitting
     - high $J_{train}(\theta)$ and high $J_{cv}(\theta)$
   - High variance: overfitting
     - low $J_{train}(\theta)$  but high $J_{cv}(\theta)$

2. Interaction with regularization:
   - Improper $\lambda$:
     - large $\lambda$ => high bias
     - small $\lambda$ => high variance
   - Choosing $\lambda$:
     - try $\lambda=0,0.01,0.02,0.04,...,10$
     - select the model with lowest $J_{cv}(\theta)$ without regularization term

3. Interaction with training set size:
   - Normal Learning curve:

      ![Normal learning curve](.\Normal learning curve.png) 

   - Learning curve with high bias:

     - where getting more training data **doesn't** help

      ![Learning curve with high bias](.\Learning curve with high bias.png) 

   - Learning curve with high variance:

     - where getting more training data **helps**

      ![Learning curve with high variance](.\Learning curve with high variance.png) 

4. Ways to fix:

   - High bias:
     - more features / more polunomial terms of features
     - decreasing $\lambda$


-    High variance:

     - larger data set
     - fewer features
     - increasing $\lambda$

- **In neural network:**

     - High bias => larger neural networks (more hidden layers / more units in one layer)
     - High variance => smaller neural networks

       **Larger network with regularization ($\lambda$) is more powerful**


#### Error Analysis

1. Procedure:
   - Algorithm (trained) misclassifies $n$ data in cross validation set
   - Classify these $n$ data and rank them
   - Maybe more features are found
2. Feature selection => Numerical evaluation
   - => test algorithm with / without this feature on **CV set** (compare error rate)

#### Skewed classes

1. Precision / Recall

   - |                 | **Actual 1**   | **Actual 0**   |
     | --------------- | -------------- | -------------- |
     | **Predicted 1** | True positive  | False positive |
     | **Predicted 0** | False negative | True negative  |

   - **Precision** = $\displaystyle \frac{\text {True positive}}{\text{#Predicted positive}} = \frac{\text {True positive}}{\text{True pos + False pos}}$

   - **Recall** = $\displaystyle \frac{\text{True positive}}{\text{#Actual positive}} = \frac{\text{True positive}}{\text{True pos + False neg}}$

2. Evaluation with precision/recall

   - Predict 1 if $ h_\theta(x) \geq \epsilon$, 0 if $h_\theta(x) < \epsilon$

     - larger $\epsilon$ => higher precision, lower recall $\small \text{(more confident)}$ 
     - smaller $\epsilon$ => lower pecision, higher recall $\small \text{(avoid missing)}$       

     ![Posiible Precision -Recall curev](.\Posiible Precision -Recall curev.png)  

3. Compare precision/recall num

   - $\displaystyle \text{F}_1 \space  Score = 2\frac{PR}{P+R}$, $P$ as precision, $R$ as recall
     - higher better, on cross validation set

4. High precision & high recall:

   - **large num of features $\small\text{(low bias)}$ + large sets of data $\small\text{(low variance)}$**


### 4. Latent Variable Analysis

#### Principal Component Analysis (PCA)

1. Motivation:

   - Data compression (reduce highly related features)
   - Data visualization

2. Assumption:

   - Gaussian distributions for both the latent and observed variables

3. Two Equivalent Definition of PCA:

   - Linear projection of data onto lower dimensional linear space (principal subspace) such that: 

     $\Rightarrow \text{variance of projected data is maximized} \\ \Rightarrow \text{distortion error from projection is minimized} $ 

4. Maximum Variance Formulation

   - Goal: 

     - project data from D dimension to M while maximizing the variance of projected data

   - Eigenvalues $\lambda$ of covariance matrix $S$ express the variance of data set $X$ in direction of corresponding eigenvectors

   - Projection Vectors: 

     - $U = (u_1,...,u_M), \text{ where } \forall i\in\{1,..,M\},u_i \in \mathbb R^D \text{ s.t. } u_i^Tu_i=1 \space \small \text{(only consider direction)} $ 

   - Projected Data:
     - $\displaystyle \text{Mean} = {\bar x}^TU \text{, where } \bar x = \frac 1 N\sum_{i=1}^Nx^i$ 
     - $\displaystyle \text{Variance} =  tr\{U^T S U\} \text{, where } S = \sum_{i=1}^N(x^i-\bar x)(x^i-\bar x)^T \space\small\text{ (outer product)}$ 

   - Lagrangian to maximize $\text{Variance}$: 

     - $\displaystyle L(U,\lambda)=tr\{U^TSU\}+tr\{(I-U^TU)\lambda\}$ 

       ​	$\small\text{constraint }u_i^Tu_i=1 \text{ to prevent $u_i \rightarrow +\infty$ } $ 

       $\text{For each $u_i\in U$, } \displaystyle \frac \partial {\partial u_i}L = 2Su_i-2\lambda_iu_i=0 \\ \Rightarrow Su_i=\lambda_iu_i \\ \Rightarrow \text{$U$ consists of eigenvectors corresponding to the first M large eigenvalue of $S$}$ 

       ​	$\small\text{($S$ symmetric $\Rightarrow U$ orthogonal)}$ 

5.  Minimum Error Formulation:

   - Introduce Orthogonal Basis Vector for D dimension:

     - $U=(u_1,...,u_D)$ 

   - Data representation:

     - Original: $\displaystyle x^n = \sum_{i=1}^D\alpha^n_iu_i$ 
     - Projected: $\displaystyle \widetilde {x^n} = \sum_{i=1}^Mz_i^nu_i+\sum_{i=M+1}^Db_iu_i \\ \small\text{$(z_1^n,...,z_M^n)$ is different for different $x^n$, $(b_{M+1},...,b_D)$ is the same for all  $x^n$}$ 

   - Cost function: $\displaystyle J=\frac1N \sum^N_{n=1}\|x^n-\widetilde{x^n}\|^2, \text{where } \widetilde{x^n}=\sum_{i=1}^Mz^n_iu_i + \sum_{i=M+1}^Db_iu_i$ 

     - $\text{Let } \begin{cases} \displaystyle \frac \partial {\partial z^n_j} J=0 \\ \displaystyle \frac \partial {\partial b_j}J=0 \end{cases} \Rightarrow \begin{cases} \displaystyle \frac 1 N 2(x^n-\widetilde{x^n})^T (-u_j) = \frac 2 N (z_j-(x^n)^Tu_j)=0 \\ \displaystyle \frac 1 N \sum_{n=1}^N2 (x^n-\widetilde {x^n})^T(-u_j) = \frac 2 N \sum_{n=1}^N (b_j-(x^n)^Tu_j)=0 \end{cases}$ 

       $\Rightarrow \begin{cases} z_j=(x^n)^Tu_j & j\in\{1,...,M\}\\ b_j=\overline{x}^Tu_j & j\in\{M+1,...,D\} \end{cases}$ 

       Noticing $\displaystyle (x^n)^Tu_j=(\sum_{i=1}^D\alpha_i^nu_i^T)u_j = a_j \Rightarrow a_j = (x^n)^Tu_j$ 

       $\displaystyle \Rightarrow x^n-\widetilde{x^n} = \sum_{i=M+1}^D[(x^n-\overline x)^Tu_i]u_i$ 

     - $ \begin{align} \boldsymbol \Rightarrow \displaystyle J &= \frac 1 N \sum_{n=1}^N\space \left( \sum_{i=M+1}^D [(x^n-\overline x)^Tu_i]u_i\right)^T \left(\sum_{i=M+1}^D [(x^n-\overline x)^Tu_i]u_i\right) \\ &= \frac 1 N \sum_{n=1}^N \left( \sum_{i=M+1}^Du_i^T ((x^n-\overline x)^Tu_i)\right) \left( \sum_{i=M+1}^D ((x^n-\overline x)^Tu_i)u_i \right) \\ &= \frac 1 N \sum_{n=1}^N \sum_{i=M+1}^D u_i^T(x^n-\overline x)^Tu_iu_i^T(x^n-\overline x)u_i & u_i \text{ orthogonal to each other} \\ &= \sum_{i=M+1}^D u_i^T \left( \frac 1 N \sum_{n=1}^N(x^n-\overline x)^T(x^n-\overline x)\right) u_i & \|u_i\|=1 \\ \end{align} \\ \displaystyle \boldsymbol \Rightarrow J =  \sum_{i=M+1}^D u_i^TSu_i \text{, where }S=\frac 1 N\sum_{n=1}^N(x^n-\overline x)^T(x^n-\overline x) $ 

   - Lagrangian to Minimize $J$: 

     - $\displaystyle L(u_{M+1},...,u_D,\lambda_{M+1},...,\lambda_D) = \sum_{i=M+1}^Du_i^TSu_i + \sum_{i=M_1}^D \lambda_i(1-u_i^Tu_i)$ 

       ​	$\small\text{constraint $\|u_i\|=1$ to prevent $u_i=0$}$ 

       $\text{For each }u_i, \displaystyle \frac \partial {\partial u_i}L=2Su_i-2\lambda_iu_i=0 \\ \Rightarrow Su_i=\lambda_iu_i \\ \Rightarrow \text{To minmize $J$, take eigenvectors with the first $(D-M)$ small eigenvalue orthogonal to (out of) subspace} \\ \Leftrightarrow \text{define subspace with eigenvectors with the first $M$ large eigenvalue}$ 

   - $\displaystyle \begin{align} \text{Intuition: }\widetilde{x_n}&=\sum_{i=1}^M((x^n)^Tu_i)u_i+\sum_{i=M+!}^D(\overline {x}^Tu_i)u_i \\ &= \overline x + \sum_{i=1}^M[(x^n-\overline x)^Tu_i]u_i \end{align}$ 


6.  Singular Value Decomposition - SVD:

   - Intorduce matrix $A_{m\times n}$ 

     - $(A^TA)_{n\times n}$ symmetric matrix ($\text{actually, Gram matrix} \rightarrow \text{semi-definite}$) 
     - $\text{eigenvalue decomposition: } \\ A^TA=VDV^T\text{, $V$ is normalized ($v_i^Tv_i=1$) with column as eigenvector}$ 

   - $AV=(Av_1,...,Av_n)_{m\times n}$ 

     - Let $r(A)=r \\ \begin{align} \Rightarrow & r(A^TA)=r(A)=r \space \\&  r(AV)=\min\{r(A),r(V)\}=\min\{r,n\}=r\end{align} $ 
     - Reduce $AV$ to basis $(Av_1,...,Av_r)$  

   - Let $\displaystyle U=(u_1,...,u_r) = (\frac {Av_1} {\sqrt {\lambda_1}},...,\frac{Av_r}{\sqrt{\lambda_r}}) \space \text{, $\lambda_i$ is $i$-thh eigenvalue of $A^TA$}$ 

     - Orthogonal: $\forall i\neq j, u_i^Tu_j=\frac 1 {\sqrt{\lambda_i\lambda_j}}v_i^TA^TAv_j=\frac {\lambda_j} {\sqrt{\lambda_i\lambda_j}}v_i^Tv_j=0$ 

     - Unit: $\|u_i\|=\frac {\|Av_i\|} {\sqrt{\lambda_i}}=\frac {\sqrt {<Av_i,Av_i>}} {\sqrt{\lambda_i}}=1$ 

       $\boldsymbol \Rightarrow U \text{ is standard orthogonal (orthonormal) basis}$ 

     - $AV=U\Sigma,\text{ where } \Sigma = D^{\frac 1 2}$ 

   - Expand $U$ to orthonormal in $\mathbb R^m: \space (u_i,...,u_m)$ 

     - Epand corresponding part in $\Sigma$ with $0$ 

   - $A = U\Sigma V^T,\text{ with singular value in $\Sigma$ in decreasing order}$  

7.  SVD with PCA:

   - $X$ is data matrix in row ($\text{centered - zero mean}$)

   - Eigenvectors of corvariance matrix $S=X^TX$ are in $V, \text{ where }X = U\Sigma V^T$ 

     - When using $S=U\Sigma V^T \Rightarrow \space U=V \space \land \space S=V\Sigma V^T$ 

       ​	$\text{reduced to eigenvalue decomposition}$ 

     - $S=VDV^T \text{ with $V$ orthonormal}$: 

       Eigenvalues $\lambda$ of covariance matrix $S$ express the variance of data set $X$ in direction of corresponding eigenvectors

   - Projection:

     - $\widetilde X = XV_M, \text{ where $V_M$ contains first M-large eigenvectors}$ 
     - Projection direction is **not** unique 

8. Reconstruction (approximate): 

   - Data is projected onto $k$ dimension using $\text{SVD}$ with $S = U\Sigma V^T$ 
   - $x_{approx} = U_{reduce} \cdot z$,  $U_{reduce}$ is n*k matrix, $z$ is k\*1 vector
   - ![Reconsturction from data Compression](.\Reconsturction from data Compression.png) 

9. Choosing $k$ (num of principal components):

   - choose the **smallest** k making $\displaystyle \frac JV \leq 0.01$ => 99% of variance is retained 

   - $[U,S,V]$ = svd(Sigma) => $\displaystyle \frac JV=1-\frac {\sum^k_{i=1}S_{ii}}{\sum^n_{i=1}S_{ii}}$, $S$ is diagonal matrix

     => check $\frac JV$ before compress data 

10. Data Preprocessing:

  - PCA $vs.$ Normalization:
    - Normalization: Individually normalized but still correlated
    - PCA: create decorrelated data $-\text{ whitening}$ 
  - Whitening: $\text{ projection with normalization}$ 
    - $S = VDV^T \text{, where $S$ is Gram matrix over $X^T$}$ 
    - $\forall n, y_n=D^{-\frac12}V^T(x^n-\overline x) \text{, where $\overline x$ is the mean of $X$} \\ \begin{align} \Rightarrow & \text{{$y^n$} has zero mean} \\ & \displaystyle cov(\{y^n\}) = \frac 1 N \sum_{n=1}^Ny_ny_n^T= D^{\frac {-1} 2}V^TSVD^{\frac{-1}2}=I \end{align}$ 

11. Tips for PCA:

    - **Don't** use PCA to prevent overfitting, use regularization instead
    - Try original data before implement PCA
    - Train PCA only on trainning set


#### Independent Component Analysis (ICA)

1. Goal:
   - Recover original signals from a mixed observed data
     - Source signal $S\in \mathbb R^{N\times K}$; mixing matrix $A$; Observed data $X=SA$
   - Maximizes statistical independence
     - Find $A^{-1}$ to maximizes independence of columns of $S$
2. Assumption: 
   - At most one signal is Gaussian distributed
   - Ignorde amplitude and order of recovered signals
   - Have at least as many observed mixtures as signals
   - $A$ invertible
3. Independence $vs.$ Uncorrelatedness
   - Independence $\Rightarrow$ Uncorrelatedness
     - $p(x_1,x_2)=p(x_1)p(x_2) \Rightarrow \mathbb E(x_1x_2)-\mathbb E(x_1)\mathbb E(x_2) = 0$ 
4. Central Limit Theorem
5. FastICA algorithm

#### t-SNE

1. Problem & Focus
2. Compared to PCA:
   - No whitening function to use for new data
   - PCA can only capture linear structure inside the data
   - t-SNE preserves the <u>local distances</u> in the original data

#### Anomaly Detection

1. Problem to solve:
   - Given dataset {$x^1,x^2,...,x^m$}, build density estimation model $p(x)$
   - $p(x^{test}<\epsilon)$ => $x^{test}$ anomaly 

2. Hypothesis function: 

   - $\displaystyle p(x)=\prod^n_{i=1} p(x_i), \space x \in R^n,\forall i \in [1,n], \space x_i \sim N(\mu_i,\sigma_i^2)$ 
     - $\displaystyle \mu=\frac1m \sum^m_{i=1}x^i,\space \sigma^2=\frac1m \sum^m_{i=1}(x^i-\mu)^2$ 
     - assume $x_1,...,x_n$ independent from each other

3. Multivariate Gaussian:

   - $\displaystyle p(x;\mu,\Sigma)=\frac1{(2\pi)^{\frac n2} |\Sigma|^{\frac 12}} exp(-\frac12 (x-\mu)^T \Sigma^{-1} (x-\mu)),$  

     $x\in R^n, \mu\in R^n,\Sigma\in R^{n\times n}$, where $\Sigma$ is covariance matrix

     - $\displaystyle \mu=\frac1m \sum^m_{i=1}x^i,\space \Sigma=\frac 1m \sum^m_{i=1}(x^i-\mu)(x^i-\mu)^T$ 
     - $x_1,...x_n$ can be correlated but **not** linearly dependent
     - need $m > n$ $(m\ge10n\space suggested)$ or elas $\Sigma$ non-invertible

4. Algorithm:

   - choose features
   - compute $\mu$, $\sigma$
   - compute $p(x)$ for new example, anomaly if $p(x) < \epsilon $ 

5. Evaluation (real-number):

   - Labeled data into normal/anomalous set

     (okay if some anomalies slip into normal set)

     - training set: unlabeled data from normal set (60%)
     - CV set: labeled data from normal (20%) & anomalous (50%) set
     - test set: labeled data from normal (20%) & anomalous (50%) set

   - Use evaluation metrics (skewed data)

6. When to use:

   - Anomaly detection:
     -  Very small num of positive data (0-20 commonly); Large num of negative data
     -  Difficult to learn from positive data (not enough data, too many features...)
     -  Future anomalies may look nothing like given data
   - Supervised Learning:
     - Larger num of positive & negative data
     - Enough positive data for algorithm to learn
     - Future positive example is likely to be similar to given data

7. Example:

   -  Anomaly detection:
   -  Fraud detection, Manufacturing, Monitoring machines in data center...
   -  Supervised learning:
      - Email spam classification (enough data), Weather prediction (sunny/rainy/etc), Cancer classification...

8. Tips:

   - Non-guassian feature: transformation / using other distribution
   - Choosing features: compare anomaly data with normal data



#### Recommender System

1. Problem Formulation:

   - $r_{i,j}=1$ if item $i$ is rated by user $j$ 

   - $y_{i,j}$ = rating of item $i$ given by user $j$ 

   - $\theta^j$ = parameter vector for user $j$ 

   - $x^i$ = feature vector for movie $i$ 

     => for user $j$, movie $i$, ($r_{i,j}=0$), predict rating $x^i\theta^j$

2. Content Based Recommendations:

   - Treat each user as a seperate linear regression problem with the feature vectors of its rated items as traning set

     **Assume features for each items ($x^i$) are available and known**

     => given $X$ estimate $\Theta$ 

   - Cost Function for $\theta_j$: 

     $\displaystyle J(\theta^j)= \frac1 {2} \sum_{i:r_{i,j}=1}(x^i\theta^j-y_{i,j})^2+\frac \lambda {2} \sum_{k=1}^n(\theta^j_k)^2, \theta^j \in R^{n+1} (\theta_0 \text{ not regularized)}$

   - Cost Function for $\Theta$:

     $\displaystyle J(\Theta)= \frac1{2} \sum_{j=1}^{n_u} \sum_{i:r_{i,j}=1}(x^i\theta^j-y_{i,j})^2+\frac \lambda {2} \sum_{j=1}^{n_u} \sum_{k=1}^n(\theta^j_k)^2, \\ \theta^j \in R^{n+1} (\theta_0 \text{ not regularized)}, \space n_u \text{ is num of users}$ 

   - Update Rule: $\forall \theta^j_k \in \theta^j, \theta^j_k := \theta^j_k-\alpha\large\frac{\partial J(\Theta)}{\partial\theta^j_k}$, $\displaystyle \frac{\partial J(\Theta)}{\partial\theta^j_k}=\sum_{i:r_{i,j}=1}(x^i\theta^j-y_{i,j})x_k^i+ \lambda \theta_k^j, \space \text{for $k \neq 0$ ($\theta^j \in R^{n+1}$)}$ 

3. Collaborative Filtering

   - **Assume preference of each users ($\theta^j$) are available and known**

     => given $\Theta$ estimate $X$ 

     - Cost Function for $x^i$: $\displaystyle J(x^i)=\frac 1 2 \sum_{j:r_{i,j}=1} (x^i\theta^j - y_{i,j})^2 + \frac \lambda 2 \sum ^n_{k=1} (x_k^i)^2$ 
     - Cost Function for $X$: $\displaystyle J(X)=\frac 1 2 \sum_{i=1}^{n_m} \sum_{j:r_{i,j}=1} (x^i\theta^j - y_{i,j})^2 + \frac \lambda 2 \sum_{i=1}^{n_m} \sum ^n_{k=1} (x_k^i)^2 \\ x^j \in R^{n+1} (x_0 \text{ not regularized)}, \space n_m \text{ is num of items}$ 
     - Update Rule: $\forall x^i_k \in x^i, x^i_k := x^i_k-\alpha\large\frac{\partial J(X)}{\partial x^i_k}$, $\displaystyle \frac{\partial J(X)}{\partial x^i_k}=\sum_{j:r_{i,j}=1}(\theta^jx^i-y_{i,j})\theta_k^j+ \lambda x_k^i, \space \text{for $k \neq 0$ $(x^i \in R^{n+1}$)}$ 

   - Basic Idea: 

     - Randomly initialize $\Theta$ 

     - loop:

       ​	Estimate $X$ 

       ​	Estimate $\Theta$ 

   - Cost Function: 

     $\displaystyle J(X,\Theta) = \frac 1 2 \sum_{(i,j):r_{i,j}=1}(x^i\theta^j - y_{i,j})^2 + \frac \lambda 2 \sum_{i=1}^{n_m}\sum_{k=1}^n(x_k^i)^2 + \frac \lambda 2 \sum_{j=1}^{n_u}\sum_{k=1}^n (\theta_k^j)^2, \space x \in R^n, \space \theta \in R^n$ 

     (the sum term in $J(\Theta)$, $J(X)$, and $J(X,\Theta)$ is the same)

   - Update Rule: 

     - $\forall x^i_k \in x^i, x^i_k := x^i_k-\alpha\large\frac{\partial J(X,\Theta)}{\partial x^i_k}$, $\displaystyle \frac{\partial J(X,\Theta)}{\partial x^i_k} = \frac{\partial J(X)}{\partial x^i_k} =\sum_{j:r_{i,j}=1}(\theta^jx^i-y_{i,j})\theta_k^j+ \lambda x_k^i, \space x^i \in R^n$ 
     - $\forall \theta^j_k \in \theta^j, \theta^j_k := \theta^j_k-\alpha\large\frac{\partial J(X,\Theta)}{\partial\theta^j_k}$, $\displaystyle \frac{\partial J(X,\Theta)}{\partial\theta^j_k} = \frac{\partial J(\Theta)}{\partial\theta^j_k} = \sum_{i:r_{i,j}=1}(\theta^jx^i-y_{i,j})x_k^i+ \lambda \theta_k^j, \space \theta^j \in R^n$ 

   - **Algorithm**: 

     - Initialize $X, \Theta$ to **small random values**

       => for symmetry breaking (similar to random initialization in neural network) 

       => so that algorithm learns features $x^1,...,x^{n_m}$ that are different from each other

     - Minimize $J(X,\Theta)$ 

     - Predict $y_{i,j} = x^i\theta^j$ ($Y = X\Theta$)

   - Finding Related Item to Recommend

     - $||x^i-x^j||$ is samll => item $i$ and $j$ is similar

   - Mean Normalization:

     - Problem: if user $j$ hasn't rated any movie, $\theta^j = [0,...,0]$  

       => predicted rating of user $j$ on all item $=0$ 

       => useless prediction

     - Algorithm (row version):

       ​	compute vector $\mu, \space \forall \mu_i \in \mu, \mu_i = \text{mean of $Y_i$, where $Y_i$ is the $i^{th}$ row in $Y$}$ 

       ​	manipulate $Y$: $\forall y_{i,j} \in Y \land r_{i,j}=1, \space y_{i,j} -= \mu_i$  => the mean of each row in $Y$ is $0$ 

       ​	predict rating for user $j$ on item $i = x^i\theta^j + \mu_i$ 

     - For item $i$ with no rating

       => apply column version of mean normalization

       (but user with no rating is generally more important)


### 5. Large Scale Machine Learning

#### Gradient Descent with Large Dataset

1. Stochastic Gradient Descent

   - Problem in Big Data: 
     - Updating $\theta$ becomes computationally expensive in batch gradient decent


- Cost Function: 
     - Cost function on single data: $cost(\theta, (x^i,y^i)) = \frac 1 2 (h_\theta(x^i)-y^i)^2$ 
     - Overall Cost Function: $\displaystyle J_{train}(\theta) = \frac 1 m \sum_{i=1}^m cost(\theta, (x^i,y^i))$


-    Procedure:

     - Randomly shuffle dataset

     - Repeat

       ​	for $i \in [1,m]$ 

       ​		$\displaystyle \begin{split} \theta_j &= \theta_j - \alpha \frac {\partial }{\partial \theta_j} cost(\theta,(x^i,y^i)) \\&= \theta_j - \alpha \space (h_\theta(x^i)-y^i) \cdot  x^i_j \end{split} \\ \text{(for $j=0,...,n$)} \\ \small => \text{make progress with each single data} $ 

- Convergence:

     - Wanting $\theta$ to converge => slowly decrease $\alpha$ over time (but more parameters)

       $(\displaystyle \text{E.g } \alpha=\frac {\text{const_1}}{\text{iteration# + const_2}})$ 

     - Compute $cost(\theta,(x^i,y^i))$ before updating 

       For every $k$ update iterations, plot average $cost(\theta,(x^i,y^i))$ over the last $k$ examples

     - Checking curves:

       Increasing $k$ result in smoother line and less noise, but the result is more delayed

       Use smaller learning rate $\alpha$ will generally have slight benefit

       Curve goes up => smaller $\alpha$ 

- $vs$ Batch Gradient Descent:

     -  use $1$ example un each update iteration => make progress earlier => faster
     -  Result may not be the optimal but in its neighbourhood

2. Mini-batch Gradient Descent

   - Use $b$ examples in each update iteration
   - $vs$  Batch Gradient Descent:
     - start to make progress earlier => faster
     - Result may not be the optimal but in its neighbourhood
   - $vs$ Stochatistic Gradient Descent:t
     - can partially parallelize computation over $b$ examples => faster under a good vectorized implementation & appropriate $b$ 
     - introduce extra parameter $b$ 

#### Online Learning

1. Situation:
   - Has too many data (can be considered as infinite)
   - When data comes in as a continuous stream
   - Can adapt to changing user preference
2. Procedure:
   - Use one example only once (Similar to stochastic gradient decent in this sense

#### Map-reduce

1. In Batch Gradient Descent:

   - Update rule $\displaystyle \theta_j = \theta_j - \alpha \frac 1 m \sum^m_{i=1} (h_\theta(x^i)-y^i)x_j^i$ 
   - Parallelize the computation of $\displaystyle \sum^m_{i=1} (h_\theta(x^i)-y^i)x_j^i$ by dividing the data set into multiple sections

2. Ability to reduce:

   - Contain operation over the whole data set

     (Neural Network can be map-reduced)


### 6. Building Machine Learning System

- Under the example of Photo OCR (Optical Character Recognition)

#### Pipeline

1. Break ML system into modules

2. Example:

   - Image -> Text detection -> Character segmentation -> Character recognition

   - Text Detection: 

     - Sliding window detection: 

       ​	set different sizes of the window (mostly rectangle), for each size:

       ​		take a image patch

       ​		resize the patch into desired size

       ​		run ML algorithm on the small patch

       ​		slide the window by $\text{step_size}$  (eventually through the image)

     - Expansion: expand the related region to create a bigger region

   - Chraracter Segmentation:

     - 1-D sliding window

   - Character Recognition

#### Getting More Data

1. **Artificial Data Synthesis** 

   - Creating New Data: Use available resource and combine them
     - Example (in Character Recognition): Paste different fonts in the randomly chosen backgrounds
   - Amplify Data Set: Intorduce distortions to the original data set
     - Need to identify the appropriate distortion
     - Usually adding purely/random/meanless noise


- Prerequisite:
     - Having a low bias/high variance hypothesis is 

2. Collect/Label Data Manually

   - Usually a surprise to find how little time it needs to get 10,000 data
     - Caculate the time it needs before decide to/not to collect the data

#### Ceilling Analysis

1. Aim:

   - Decide which modules might be the best use of time to improve

2. Procedure:

   - Draw a table with 2 column (Component - Accuracy)

     - Component: the modules simulated to be perfect (100% accuracy)
     - Accuracy: the accuracy of the entire system on the test set (define by chosen evaluation matrix)

   - |   **Perfect Component**   |             **Accuracy**              |
     | :-----------------------: | :-----------------------------------: |
     |           none            |                  $f$                  |
     |        module $1$         |            $f+\epsilon_1$             |
     |       module $1,2$        |       $f+\epsilon_1+\epsilon_2$       |
     | $\cdot \\ \cdot \\ \cdot$ |       $\cdot \\ \cdot \\ \cdot$       |
     |    module $1,2,...,n$     | $f+\epsilon_1+...+\epsilon_n = 100\%$ |

   - => Improving module $x$ will gain at most $\epsilon_x$ improvement in the overall performance

   - Choose the module with most significant $\epsilon$ to improve










