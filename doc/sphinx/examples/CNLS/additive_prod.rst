=====================================
Production function: additive model 
=====================================

Hildreth (1954) was the first to consider nonparametric regression subject to 
monotonicity and concavity constraints in the case of a single input variable :math:`x`. 
Kuosmanen (2008) extended Hildreth’s approach to the multivariate setting with a 
vector-valued :math:`\bf{x}`, and coined the term convex nonparametric least squares (``CNLS``) for this method.
``CNLS`` builds upon the assumption that the true but unknown production function 
:math:`f` belongs to the set of continuous, monotonic increasing and globally concave functions, 
imposing exactly the same production axioms as standard DEA. 

The multivariate ``CNLS`` formulation is defined as:

.. math::
    :nowrap:
    
    \begin{align*}
      & \underset{\alpha, \beta, \varepsilon} {min} \sum_{i=1}^n\varepsilon_i^2 \\
      & \text{s.t.} \\
      &  y_i = \alpha_i + \beta_i^{'}X_i + \varepsilon_i \quad \forall i \\
      &  \alpha_i + \beta_i^{'}X_i \le \alpha_j + \beta_j^{'}X_i  \quad  \forall i, j\\
      &  \beta_i \ge 0 \quad  \forall i \\
    \end{align*}

where :math:`\alpha_i` and :math:`\beta_i` define the intercept and slope parameters of 
tangent hyperplanes that characterize the estimated piece-wise linear frontier. 
:math:`\varepsilon_i` denotes the CNLS residuals. The first constraint can be interpreted 
as a multivariate regression equation, the second constraint imposes convexity, 
and the third constraint imposes monotonicity.


Example `[.ipynb] <https://colab.research.google.com/github/ds2010/pyStoNED/blob/master/notebooks/CNLS_prod.ipynb>`_
------------------------------------------------------------------------------------------------------------------------------

In the following code, we estimate an additive production function with pyStoNED.

.. code:: python

    # import packages
    from pystoned import CNLS
    from pystoned.constant import CET_ADDI, FUN_PROD, OPT_LOCAL, RTS_VRS
    from pystoned.dataset import load_Finnish_electricity_firm
    
    # import Finnish electricity distribution firms data
    data = load_Finnish_electricity_firm(x_select=['Energy', 'Length', 'Customers'],
                                          y_select=['TOTEX'])

    # define and solve the CNLS model
    model = CNLS.CNLS(y=data.y, x=data.x, z=None, cet = CET_ADDI, fun = FUN_PROD, rts = RTS_VRS)
    
    # estimate the model: 1) local estimation; 2) remote estimation
    model.optimize(OPT_LOCAL)

    # Please replace with your own email address reqired by NEOS server (see https://neos-guide.org/content/FAQ#email)
    # model.optimize('email@address') 

    # display the estimates
    model.display_alpha()
    model.display_beta()
    model.display_residual()

    # store the estimates
    alpha = model.get_alpha()
    beta = model.get_beta()
    residuals = model.get_residual()
