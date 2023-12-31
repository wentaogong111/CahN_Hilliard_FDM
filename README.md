# Cahn_Hilliard_FDM
[reference](https://github.com/wentaogong111/Numerical_methods/blob/main/Reports/Application%20of%20Numerical%20Schemes%20%26%20Assignment.pdf)

The python script to solve the Cahn-Hilliard equation using finite difference method (FTCS) with 3 different type boundary conditions


__Cahn-Hilliard Equation:__ This equation models the time evolution of a scalar field $\phi$ which is a fourth-order partial differential equation that models phase separation. Its basic form in real space can be written as:


$$\frac{\partial \phi(x, t)}{\partial t} = \nabla^2 \left( \nabla^2 \phi(x, t) - f'(\phi) \right)$$


Where $f'(\phi)$ is the derivative of the free energy density with respect to $\phi$), the order parameter.

Phase-field model for liquid-liquid phase separation

$$ \frac{\partial c}{\partial t} = M \nabla^2(c^3-c-\kappa\nabla^2c) $$

where c is the concentration of binary mixture and can have values between -1 and 1, where -1 corresponds to the pure presence of one compoent, 1 corresponds to the pure presence of the other components

1. $\frac{\partial c}{\partial t}$:the rate of change of the concentration with time

2. $\mu$ is the chemnical potential represented by $c^3-c-\kappa\nabla^2c$


This equation descripes how the mixtures evolves while it tries its local free energy $\frac{\partial f}{\partial c}= c^3-c$ and interfaces $\kappa\nabla^2c$ between the two componment 

![Cahn-Hilliard Equation](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/equation.png)

# Finite Difference Method:Forward Time Central Space(FTCS),Explicit 

![png](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/grid.png)


$$ \frac{c_{n+1}-c_{n}}{\Delta t}= M\nabla^2\mu $$

$$ \mu = c^3-c-\kappa\nabla^2c $$

$$ 
\nabla^2\mu = \frac{\mu_{i+1,j}-2\mu_{i,j}+\mu_{i-1,j}}{(\Delta x)^2}+\frac{\mu_{i,j+1}-2\mu_{i,j}+\mu_{i,j-1}}{(\Delta y)^2}
$$

$$ 
\Delta x = \Delta y 
$$

$$
c_{i,j}^{n+1}=c_{i,j}^{n}+\frac{\Delta t M}{(\Delta x)^2}(\mu_{i+1,j}+\mu_{i-1,j}+\mu_{i,j+1}+\mu_{i,j-1}-4\mu_{i,j})
$$

$$ 
\mu_{i,j} = (c_{i,j}^{n})^3-c_{i,j}^{n}-\frac{\kappa}{(\Delta x)^2}(c_{i+1,j}+c_{i-1,j}+c_{i,j+1}+c_{i,j-1}-4c_{i,j})
$$
## Note

The FTCS scheme is conditionally stable for certain types of problems. The stability condition for diffusion equation is $\alpha \frac{\Delta t}{\Delta x^2} \le \frac{1}{2}$

 However, the presence of nonlinear terms and additional components related to $\kappa$ and $c^3$ complicates the stability condition.

## Calculation Steps 
Main loop for time evolution
1. Update $\mu$ field based on current c field on the domain 

 $$ 
\mu_{i,j} = (c_{i,j}^{n})^3-c_{i,j}^{n}-\frac{\kappa}{(\Delta x)^2}(c_{i+1,j}+c_{i-1,j}+c_{i,j+1}+c_{i,j-1}-4c_{i,j})
$$

 2. Update c field based on $\mu$ field on the domian
 $$ 

\nabla^2\mu = \frac{\mu_{i+1,j}-2\mu_{i,j}+\mu_{i-1,j}}{(\Delta x)^2}+\frac{\mu_{i,j+1}-2\mu_{i,j}+\mu_{i,j-1}}{(\Delta y)^2}
$$


$$
c_{i,j}^{n+1}=c_{i,j}^{n}+\frac{\Delta t M}{(\Delta x)^2}(\mu_{i+1,j}+\mu_{i-1,j}+\mu_{i,j+1}+\mu_{i,j-1}-4\mu_{i,j})
$$

3. Store the updated c field for the next time step

# Example

The following figure is a result for the system with M= 1.0. dx=1.0,dt - 0.01, $\kappa=0.2$ The initial condition is given by a normal distribution 
$$
c(\boldsymbol{r},t=0) = c_0 + 0.1 \mathcal{N}(0,1)
$$


And the system is evolved until N = 200 steps. 
### c0 = 0.5
![GIF](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/Code/bc-c0-Dirichlet_0.5.gif)

![GIF](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/Code/bc-c0-Neumann_0.5.gif)

![GIF](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/Code/bc-c0-Periodic_0.5.gif)

## Discretization: Runge-Kutta 4th Order (RK4) and central space method 


### Chemical Potential ($ \mu $)

$$
\mu_{i,j} = c_{i,j}^3 - c_{i,j} - \frac{\kappa}{\Delta x^2}\nabla^2 c
$$

### Time Evolution of \( c \)

$$
\frac{\partial c}{\partial t} = M \nabla^2\mu
$$

## 4th-Order Runge-Kutta Scheme

In the 4th-order Runge-Kutta method (RK4), the updating scheme has four stages, each with an intermediate step. Let's denote $ c^n $ as the value of $ c $ at the nth time step, and $ \Delta t $ as the time step size. The scheme for RK4 is:

1. $ k_1 = f(c^n) $
2. $ k_2 = f\left(c^n + \frac{\Delta t}{2} k_1\right) $
3. $ k_3 = f\left(c^n + \frac{\Delta t}{2} k_2\right) $
4. $ k_4 = f\left(c^n + \Delta t k_3\right) $

Then, the update formula is:

$$
c^{n+1} = c^n + \frac{\Delta t}{6} \left( k_1 + 2k_2 + 2k_3 + k_4 \right)
$$

Here,$ f(c) = M\nabla^2\mu$


1. Update $\mu$ field based on current c field on the domain 

 $$ 
\mu_{i,j} = (c_{i,j}^{n})^3-c_{i,j}^{n}-\frac{\kappa}{(\Delta x)^2}(c_{i+1,j}+c_{i-1,j}+c_{i,j+1}+c_{i,j-1}-4c_{i,j})
$$

 2. Update c field based on $\mu$ field on the domian

$$ 
\nabla^2\mu = \frac{\mu_{i+1,j}-2\mu_{i,j}+\mu_{i-1,j}}{(\Delta x)^2}+\frac{\mu_{i,j+1}-2\mu_{i,j}+\mu_{i,j-1}}{(\Delta y)^2}
$$

$ f(c) = M \nabla^2\left(-\left(c^3 - c\right) + \nabla^2 c\right) $.
 
 
The Laplacian in 2D can be approximated by the 5-point stencil as follows:

$$
\nabla^2 c \approx \frac{c_{i+1,j} - 2c_{i,j} + c_{i-1,j}}{\Delta x^2} + \frac{c_{i,j+1} - 2c_{i,j} + c_{i,j-1}}{\Delta y^2}
$$


By following these steps, we can discretize the 2D Cahn-Hilliard equation in time using the 4th-order Runge-Kutta scheme with 2nd order spatial discretization.

![GIF](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/Code/RK4_bc-c0-Dirichlet_0.5.gif)

![GIF](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/Code/RK4_bc-c0-Neumann_0.5.gif)

![GIF](https://github.com/wentaogong111/CahN_Hilliard_FDM/blob/main/Code/RK4_bc-c0-Periodic_0.5.gif)