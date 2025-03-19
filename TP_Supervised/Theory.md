# Réponses aux questions théoriques

## Question 1 : Estimateur OLS et variance minimale

### Calcul de l'espérance de l'estimateur 

Nous avons l'estimateur alternatif $\beta^\sim$ donné par :

$$
E(\beta^\sim) = E(Cy) = E((H + D)y)
$$

En développant :

$$
E(\beta^\sim) = E(Hy) + E(Dy) = E(\beta^*) + E(Dy) = \beta^* + D X^T \beta
$$

On sait que :

$$
E(y) = X\beta \quad \text{et} \quad E(\beta^*) = \beta
$$

D'où :

$$
E(\beta^\sim) = \left(I_d + D X^*\right) \beta
$$

Pour que l'estimateur soit sans biais, il faut que :

$$
D X^* = 0
$$

### Vérification du caractère sans biais de l'OLS

Pour montrer que $\beta^*$ est sans biais :

$$
E(\beta^*) = (X^TX)^{-1} X^T E(y) = (X^TX)^{-1} X^T X \beta = \beta
$$

L'OLS est donc bien un estimateur sans biais.

---

### Calcul de la variance de l'estimateur 

Nous avons :

$$
Var(\beta^\sim) = Var(Cy) = C \cdot Var(y) \cdot C^T
$$

Sachant que la variance du bruit est $\sigma^2$, par conséquent $Var(y) = \sigma^2 I$, on obtient :

$$
Var(\beta^\sim) = \sigma^2 C C^T = \sigma^2 (H + D)(H + D)^T
$$

En développant :

$$
Var(\beta^\sim) = \sigma^2 \left(H H^T + H D^T + D H^T + D D^T\right)
$$

En utilisant les expressions de $H$ et $D$, nous avons :

$$
Var(\beta^\sim) = \sigma^2 \left((X^TX)^{-1} + D D^T\right) = Var(\beta^*) + \sigma^2 D D^T
$$

### Comparaison des variances

Puisque $D D^T$ est une matrice semi-définie positive, alors :

$$
Var(\beta^\sim) \geq Var(\beta^*)
$$

Cela montre que la variance de $\beta^\sim$ est toujours supérieure ou égale à celle de l'OLS.

## Question 2 : Estimateur Ridge

### Calcul de l'estimateur Ridge
L'estimateur Ridge se définit comme suit :

$$
\beta_{ridge} = \left(X^TX + \lambda I_d\right)^{-1} X^Ty
$$

#### Calcul de l'espérance
Pour vérifier le biais de cet estimateur, nous calculons son espérance :

$$
E(\beta_{ridge}) = E\left(\left(X^TX + \lambda I_d\right)^{-1} X^Ty\right)
$$

En remplaçant $ y $ par $ X\beta + \epsilon $ (avec $ \epsilon $ le terme d'erreur) :

$$
E(\beta_{ridge}) = \left(X^TX + \lambda I_d\right)^{-1} X^T E(y)
$$

Comme $ E(y) = X\beta $ :

$$
E(\beta_{ridge}) = \left(X^TX + \lambda I_d\right)^{-1} X^T X \beta
$$

Nous avons donc :

$$
E(\beta_{ridge}) = \left(X^TX + \lambda I_d\right)^{-1} (X^TX) \beta
$$

Pour que l'espérance soit égale à $ \beta $, il faudrait que :

$$
\left(X^TX + \lambda I_d\right)^{-1} (X^TX) = I_d
$$

Ce qui n'est possible que si $ \lambda = 0 $.

**Conclusion :** L'estimateur Ridge est biaisé.

---

### Utilisation de la décomposition SVD

La décomposition en valeurs singulières (SVD) est utilisée ici pour simplifier le calcul de l'estimateur Ridge. Rappelons que pour une matrice $X$ de taille $n  imes p$, on peut écrire la décomposition SVD comme suit :

$$
X = U \Sigma V^T
$$

où :
- $U$ est une matrice $n \times n$ orthogonale ($U^TU = I$),
- $\Sigma$ est une matrice $n \times p$ diagonale contenant les valeurs singulières,
- $V$ est une matrice $p \times p$ orthogonale ($V^TV = I$).

L'estimateur Ridge est donné par :

$$
\beta_{R} = (X^TX + \lambda I)^{-1} X^Ty_c
$$

En remplaçant la décomposition SVD dans l'expression de l'estimateur :

$$
\beta_{R} = (V \Sigma^T U^T U \Sigma V^T + \lambda I)^{-1} V \Sigma^T U^T y_c
$$

Puisque $U$ est orthogonale ($U^T U = I$), nous avons :

$$
\beta_{R} = (V \Sigma^T \Sigma V^T + \lambda I)^{-1} V \Sigma^T U^T y_c
$$

Factorisons $V$ à gauche et à droite :

$$
\beta_{R} = V(\Sigma^T \Sigma + \lambda I)^{-1} \Sigma^T U^T y_c
$$

Étant donné que $\Sigma^T \Sigma$ est une matrice diagonale, son inverse est simple à calculer. Cela rend l'estimateur Ridge calculé par SVD plus simple et numériquement stable.

---

### Analyse des variances de Ridge et OLS

### Calcul de la variance de Ridge
L'estimateur Ridge est donné par :

$$
\beta_{Ridge}^* = V (\Sigma^2 + \lambda I_d)^{-1} U^T y_c
$$

où :
- $ V $ et $ U $ sont issus de la décomposition en valeurs singulières (SVD) de la matrice $ X $, telle que $ X = U \Sigma V^T $.
- $ \lambda $ est le paramètre de pénalisation.

La variance de cet estimateur est alors :

$$
Var(\beta_{Ridge}^*) = \sigma^2 V (\Sigma^2 + \lambda I_d)^{-2} V^T
$$

### Calcul de la variance de l'OLS
L'estimateur OLS est donné par :

$$
\beta_{OLS}^* = (X^TX)^{-1} X^T y
$$

et sa variance est :

$$
Var(\beta_{OLS}^*) = \sigma^2 (X^TX)^{-1} = \sigma^2 (\Sigma^2)^{-1}
$$

### Comparaison des variances
Nous comparons les deux variances pour montrer que :

$$
Var(\beta_{Ridge}^*) < Var(\beta_{OLS}^*)
$$

Cela revient à montrer que :

$$
(\Sigma^2 + \lambda I_d)^{-1} < (\Sigma^2)^{-1}
$$

En prenant l'inverse des deux membres, cela revient à comparer :

$$
\Sigma^2 < \Sigma^2 + \lambda I_d
$$

Ce qui est toujours vrai pour $ \lambda > 0 $, ce qui prouve la réduction de la variance avec la régularisation Ridge.

---

### Analyse de la variance de l'estimateur Ridge

En reprenant les expressions de la variance et du biais obtenues précédemment, nous avons :

### Variance de l'estimateur Ridge

$$
Var(\beta_{Ridge}) = \sigma^2 \left( (X^TX + \lambda I_d)^{-1} X^TX (X^TX + \lambda I_d)^{-1} \right)
$$

On remarque que plus le paramètre $\lambda$ augmente, plus la variance diminue. Cela signifie que l'effet de la régularisation se traduit par une réduction de la sensibilité de l'estimateur aux variations aléatoires des données.

### Biais de l'estimateur Ridge

$$
Biais(\beta_{Ridge}) = \left( (X^TX + \lambda I_d)^{-1} X^TX - I \right) \beta
$$

À mesure que $\lambda$ augmente, le biais augmente également, traduisant une déviation accrue par rapport à l'estimateur non biaisé OLS. Cependant, plus $\lambda$ augmente, plus la variance diminue, ce qui conduit à un compromis biais-variance.

### Compromis Biais-Variance

L'objectif est de trouver un équilibre optimal entre biais et variance. Une valeur trop faible de $\lambda$ conduit à une variance élevée (surdétermination) et un faible biais. À l'inverse, une valeur trop grande de $\lambda$ entraîne une faible variance mais un biais important.

---

Pour cette question, nous examinons trois estimateurs différents :

1. L'estimateur Ridge :

$$
\beta^*_{Ridge} = \left( I_d + \lambda I_d \right)^{-1} x_c^T y_c
$$

2. L'estimateur OLS (Moindres carrés ordinaires) :

$$
\beta^*_{OLS} = x_c^T y_c
$$

3. Un estimateur modifié lié à Ridge :

$$
\beta^*_{Ridge} = \left( (1 + \lambda) I_d \right)^{-1} x_c^T y_c = \frac{1}{1 + \lambda} K_{OLS}
$$

où $ K_{OLS} $ représente l'estimateur OLS.

L'interprétation clé ici est que l'estimateur modifié $ \beta^*_{Ridge} $ est simplement une version ajustée de l'OLS par un facteur d'atténuation $ \frac{1}{1 + \lambda} $. Cela signifie que plus $ \lambda $ augmente, plus la pénalisation est forte, ce qui réduit la variance mais augmente le biais.

---

Pour trouver la solution de l'Elastic Net, on minimise :

$$
J(\beta) = (y_c - x_c \beta)^T(y_c - x_c \beta) + \lambda_2 \|\beta\|^2 + \lambda_1 \|\beta\|_1
$$

avec $ x_c^T x_c = I_d $.

### Solution :
- Le gradient est nul à l'optimum :

$$
-2 x_c^T y_c + 2\beta + 2\lambda_2 \beta + \lambda_1 d_1 = 0
$$

où $ d_1 = \text{sgn}(\beta) $.  
- En isolant $ \beta $ :

$$
\beta_{ElNet} = \frac{\beta_{OLS} - \frac{\lambda_1 d_1}{2}}{1 + \lambda_2}
$$













