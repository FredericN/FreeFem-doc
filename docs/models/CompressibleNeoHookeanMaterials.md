_Written by Alex Sadovsky_

$$
\def\bR{{\bf R}}
\def\bP{{\bf P}}
\def\bZ{{\bf Z}}
\def\bC{{\bf C}}
\def\VS{\bR^2}
\def\SVS{\underline V}
\def\SO{{\bf SO}}
\def\Sym{{\bf Sym}}
\def\qi{{\bf i}}
\def\qj{{\bf j}}
\def\qk{{\bf k}}
\def\ec{\hat{\bf e}}
\def\xc{\hat{\bf x}}
\def\bdr{\partial}
\def\PD{\partial_}
\def\strain{\underline \epsilon}
\def\stress{\underline \sigma}
\def\strainrate{\underline \epsilon^.}
\def\stressrate{\underline \sigma^.}
\def\stiff{\; \underline{\underline C}\;}
\def\comply{\underline{\underline \kappa}\;}
\def\Id{{\bf I}}
\def\Div{\nabla \cdot}
\def\Grad{\mathbf{\nabla}}
\def\rot{\nabla \times}
\def\lap{\triangle}
\def\tr{{\bf tr}\;}
\def\udH{\underline H}
\def\refX{\mathbf X}
\def\Jac{\overline{J}}
\def\spatx{\mathbf x}
\def\ani{\overline a}
\def\mat{\left[\begin{array}}
\def\tam{\end{array}\right]}
\def\arr{\left.\begin{array}}
\def\rra{\end{array}\right\}}
\def\arl{\left\{\begin{array}}
\def\lra{\end{array}\right.}
\def\ar{\begin{array}}
\def\ra{\end{array}}
\def\const{\mbox{ const.}}
\def\eps{\; \epsilon}
\def\sig{\; \sigma}
\def\th{\theta}
\def\sgn{\mbox{sgn}}
\def\qed{\; Q.E.D.\\}
\def\ranqe{\end{eqnarray}}
\def\ol{\overline}
\def\ul{\underline}
\def\bB{{\bf B}}
\def\bC{{\bf C}}
\def\bD{{\bf D}}
\def\bE{{\bf E}}
\def\bF{{\bf F}}
\def\bK{{\bf K}}
\def\bP{{\bf P}}
\def\bS{{\bf S}}
\def\bT{{\bf T}}
\def\bsig{{\bf \sigma}}
$$

## Notation

In what follows, the symbols $\mathbf{u}, \bF, \bB, \bC, \stress$ denote, respectively, the displacement field, the deformation gradient, the left Cauchy-Green strain tensor $\bB = \bF \bF^T$, the right Cauchy-Green strain tensor $\bC =\bF^T \bF$, and the Cauchy stress tensor.

We also introduce the symbols $I_1 := \tr \bC$ and $J := \det\bF$. Use will be made of the identity
\begin{equation}
{\PD{}J \over \PD{}\bC} = J \bC^{-1}
\end{equation}

The symbol $\Id$ denotes the identity tensor. The symbol $\Omega_{0}$ denotes the reference configuration of the body to be deformed. The unit volume in the reference (resp., deformed) configuration is denoted $dV$ (resp., $dV_{0}$); these two are related by
$$
dV = J dV_{0},
$$
which allows an integral over $\Omega$ involving the Cauchy stress $\bT$ to be rewritten as an integral of the Kirchhoff stress $\kappa = J \bT$ over $\Omega_{0}$.

## Recommended References

For an exposition of nonlinear elasticity and of the underlying linear- and tensor algebra, see [OGDEN1984](#OGDEN1984). For an advanced mathematical analysis of the Finite Element Method, see [RAVIART1998](#RAVIART1998).

## A Neo-Hookean Compressible Material

_Constitutive Theory and Tangent Stress Measures_

The strain energy density function is given by
\begin{equation}
W = {\mu \over 2}(I_1 - \tr \Id - 2 \ln J)
\end{equation}
(see [HORGAN2004](#HORGAN2004), formula (12)).

The corresponding 2nd Piola-Kirchoff stress tensor is given by
\begin{equation}
\bS_{n} := {\PD{} W \over \PD{}\bE} (\bF_{n})
=
\mu (\Id - \bC^{-1})
\end{equation}
The Kirchhoff stress, then, is
\begin{equation}
\kappa
= \bF \bS \bF^{T}
= \mu (\bB - \Id)
\end{equation}
The tangent Kirchhoff stress tensor at $\bF_{n}$ acting on
$
\delta \bF_{n+1}
$ is, consequently,
\begin{equation}
{\PD{} \kappa \over \PD{} \bF} (\bF_{n}) \delta \bF_{n+1}
=
\mu
\left[
\bF_{n} (\delta \bF_{n+1})^T
+
\delta \bF_{n+1} (\bF_{n})^T
\right]
\end{equation}

_The Weak Form of the BVP in the Absence of Body (External) Forces_

The $\Omega_0$ we are considering is an elliptical annulus, whose
boundary consists of two concentric ellipses (each allowed to be a
circle as a special case), with the major axes parallel. Let $P$ denote the dead stress load (traction) on a portion
$\partial \Omega_0^{t}$ (= the inner ellipse) of the boundary
$\partial \Omega_0$. On the rest of the boundary, we prescribe zero displacement.

The weak formulation of the boundary value
problem is
$$
\arr{lll}
0
& = &
\int_{\Omega_0}
\kappa[\bF]
\:
:
\:
\left\{
(\Grad \otimes \mathbf{w}) (\bF)^{-1}
\right\}\\
& - & \int_{\PD{} \Omega_0^{t}} P \cdot \hat{N}_0\\
\rra
$$

For brevity, in the rest of this section we assume $P = 0$. The provided
FreeFem++ code, however, does not rely on this assumption and allows
for a general value and direction of $P$.

Given a Newton approximation $\mathbf{u}_n$ of the displacement field
$\mathbf{u}$ satisfying the BVP, we seek the correction $\delta \mathbf{u}_{n+1}$ to
obtain a better approximation
$$
\mathbf{u}_{n+1} = \mathbf{u}_{n} + \delta \mathbf{u}_{n+1}
$$
by solving the weak formulation
\begin{equation}
\arr{lll}
	0 &=& \int_{\Omega_0}\kappa[\bF_{n} + \delta \bF_{n+1}]\: :\: \left\{(\Grad \otimes \mathbf{w}) (\bF_{n} + \delta\bF_{n+1})^{-1}\right\}- \int_{\PD{} \Omega_0} P \cdot \hat{N}_0\\
	&=& \int_{\Omega_0}\left\{\kappa[\bF_{n}] + {\PD{} \kappa \over \PD{} \bF}[\bF_{n}]\delta \bF_{n+1}\right\}\: :\: \left\{(\Grad \otimes \mathbf{w})(\bF_{n} + \delta \bF_{n+1})^{-1}\right\}\\
	&=& \int_{\Omega_0}\left\{\kappa[\bF_{n}] + {\PD{} \kappa \over \PD{} \bF}[\bF_{n}]\delta \bF_{n+1}\right\}\: :\: \left\{(\Grad \otimes \mathbf{w}) (\bF_{n}^{-1} + \bF_{n}^{-2} \delta \bF_{n+1})\right\}\\
	\\
	&=& \int_{\Omega_0}\kappa[\bF_{n}]\: :\: \left\{(\Grad \otimes \mathbf{w})\bF_{n}^{-1}\right\}\\
	&-& \int_{\Omega_0}\kappa[\bF_{n}]\: :\: \left\{(\Grad \otimes \mathbf{w})(\bF_{n}^{-2} \delta \bF_{n+1})\right\}\\
	&+& \int_{\Omega_0}\left\{{\PD{} \kappa \over \PD{} \bF}[\bF_{n}]\delta \bF_{n+1}\right\}\: :\: \left\{(\Grad \otimes \mathbf{w})
\bF_{n}^{-1}
\right\}
\\
\rra
\quad
\mbox{for all test functions} \mathbf{w},
\end{equation}
where we have taken
$$
\delta \bF_{n+1} = \Grad \otimes \delta \mathbf{u}_{n+1}
$$

{\bf Note:} Contrary to standard notational use, the symbol $\delta$
here bears no variational context. By $\delta$ we mean simply an
increment in the sense of Newton's Method. The role of a variational virtual displacement here
is played by $\mathbf{w}$.

## An Approach to Implementation in FreeFem++

Introducing the code-like notation, where a string in $< >$'s is to be read as one symbol, the individual components of the tensor
\begin{equation}
<TanK>
 :=
{\PD{} \kappa \over \PD{} \bF}[\bF_{n}]
\delta \bF_{n+1}
\end{equation}
will be implemented as the macros $<TanK11>$, $<TanK12>$, ...

The individual components of the tensor quantities
$$
\bD_{1} :=
\bF_{n} (\delta \bF_{n+1})^T
+
\delta \bF_{n+1} (\bF_{n})^T,
$$
$$
\bD_{2} :=
\bF_{n}^{-T} \delta \bF_{n+1},
$$
$$
\bD_{3} :=
(\Grad \otimes \mathbf{w})
\bF_{n}^{-2} \delta \bF_{n+1},
$$
and
$$
\bD_{4} :=
(\Grad \otimes \mathbf{w})
\bF_{n}^{-1},
$$
will be implemented as the macros
\begin{equation}
\arr{l}
<d1Aux11>, <d1Aux12>, \quad \ldots \quad, <d1Aux22>,\\
<d2Aux11>, <d2Aux12>, \quad \ldots \quad, <d2Aux22>\\
<d3Aux11>, <d3Aux12>, \quad \ldots \quad, <d3Aux22>\\
<d4Aux11>, <d4Aux12>, \quad \ldots \quad, <d4Aux22>\\
\rra,
\end{equation}
respectively.

In the above notation, the tangent Kirchhoff stress term becomes
\begin{equation}
{\PD{} \kappa \over \PD{} \bF} (\bF_{n})
\: \delta \bF_{n+1}
=
\mu
\: \bD_{1}
\end{equation}
while the weak BVP formulation acquires the form
\begin{equation}
\arr{lll}
0 & = &
\int_{\Omega_0}
\kappa[\bF_{n}]
\:
:
\:
\bD_{4}
\\
&-&
\int_{\Omega_0}
\kappa[\bF_{n}]
\:
:
\:
\bD_{3}
\\
&+&
\int_{\Omega_0}
\left\{
{\PD{} \kappa \over \PD{} \bF}[\bF_{n}]
\delta \bF_{n+1}
\right\}
\:
:
\:
\bD_{4}
\\
\rra
\quad
\mbox{for all test functions} \mathbf{w}
\end{equation}

```freefem
// Macro
//Macros for the gradient of a vector field (u1, u2)
macro grad11(u1, u2) (dx(u1)) //
macro grad21(u1, u2) (dy(u1)) //
macro grad12(u1, u2) (dx(u2)) //
macro grad22(u1, u2) (dy(u2)) //

//Macros for the deformation gradient
macro F11(u1, u2) (1.0 + grad11(u1, u2)) //
macro F12(u1, u2) (0.0 + grad12(u1, u2)) //
macro F21(u1, u2) (0.0 + grad21(u1, u2)) //
macro F22(u1, u2) (1.0 + grad22(u1, u2)) //

//Macros for the incremental deformation gradient
macro dF11(varu1, varu2) (grad11(varu1, varu2)) //
macro dF12(varu1, varu2) (grad12(varu1, varu2)) //
macro dF21(varu1, varu2) (grad21(varu1, varu2)) //
macro dF22(varu1, varu2) (grad22(varu1, varu2)) //

//Macro for the determinant of the deformation gradient
macro J(u1, u2) (
	  F11(u1, u2)*F22(u1, u2)
	- F12(u1, u2)*F21(u1, u2)
) //

//Macros for the inverse of the deformation gradient
macro Finv11 (u1, u2) (
	  F22(u1, u2) / J(u1, u2)
) //
macro Finv22 (u1, u2) (
	  F11(u1, u2) / J(u1, u2)
) //
macro Finv12 (u1, u2) (
	- F12(u1, u2) / J(u1, u2)
) //
macro Finv21 (u1, u2) (
	- F21(u1, u2) / J(u1, u2)
) //

//Macros for the square of the inverse of the deformation gradient
macro FFinv11 (u1, u2) (
	  Finv11(u1, u2)^2
	+ Finv12(u1, u2)*Finv21(u1, u2)
) //

macro FFinv12 (u1, u2) (
	  Finv12(u1, u2)*(Finv11(u1, u2)
	+ Finv22(u1, u2))
) //

macro FFinv21 (u1, u2) (
	  Finv21(u1, u2)*(Finv11(u1, u2)
	+ Finv22(u1, u2))
) //

macro FFinv22 (u1, u2) (
	  Finv12(u1, u2)*Finv21(u1, u2)
	+ Finv22(u1, u2)^2
) //

//Macros for the inverse of the transpose of the deformation gradient
macro FinvT11(u1, u2) (Finv11(u1, u2)) //
macro FinvT12(u1, u2) (Finv21(u1, u2)) //
macro FinvT21(u1, u2) (Finv12(u1, u2)) //
macro FinvT22(u1, u2) (Finv22(u1, u2)) //

//The left Cauchy-Green strain tensor
macro B11(u1, u2) (
	  F11(u1, u2)^2 + F12(u1, u2)^2
)//

macro B12(u1, u2) (
	  F11(u1, u2)*F21(u1, u2)
	+ F12(u1, u2)*F22(u1, u2)
)//

macro B21(u1, u2) (
	  F11(u1, u2)*F21(u1, u2)
	+ F12(u1, u2)*F22(u1, u2)
)//

macro B22(u1, u2)(
	  F21(u1, u2)^2 + F22(u1, u2)^2
)//

//The macros for the auxiliary tensors (D0, D1, D2, ...): Begin
///The tensor quantity D0 = F_{n} (\delta F_{n+1})^T
macro d0Aux11 (u1, u2, varu1, varu2) (
	  dF11(varu1, varu2) * F11(u1, u2)
	+ dF12(varu1, varu2) * F12(u1, u2)
)//

macro d0Aux12 (u1, u2, varu1, varu2) (
	  dF21(varu1, varu2) * F11(u1, u2)
	+ dF22(varu1, varu2) * F12(u1, u2)
)//

macro d0Aux21 (u1, u2, varu1, varu2) (
	  dF11(varu1, varu2) * F21(u1, u2)
	+ dF12(varu1, varu2) * F22(u1, u2)
)//

macro d0Aux22 (u1, u2, varu1, varu2) (
	  dF21(varu1, varu2) * F21(u1, u2)
	+ dF22(varu1, varu2) * F22(u1, u2)
)//

///The tensor quantity D1 = D0 + D0^T
macro d1Aux11 (u1, u2, varu1, varu2) (
	  2.0 * d0Aux11 (u1, u2, varu1, varu2)
)//

macro d1Aux12 (u1, u2, varu1, varu2) (
	  d0Aux12 (u1, u2, varu1, varu2)
	+ d0Aux21 (u1, u2, varu1, varu2)
)//

macro d1Aux21 (u1, u2, varu1, varu2) (
	  d1Aux12 (u1, u2, varu1, varu2)
)//

macro d1Aux22 (u1, u2, varu1, varu2)(
	  2.0 * d0Aux22 (u1, u2, varu1, varu2)
)//

///The tensor quantity D2 = F^{-T}_{n} dF_{n+1}
macro d2Aux11 (u1, u2, varu1, varu2) (
	  dF11(varu1, varu2) * FinvT11(u1, u2)
	+ dF21(varu1, varu2) * FinvT12(u1, u2)
)//

macro d2Aux12 (u1, u2, varu1, varu2) (
	  dF12(varu1, varu2) * FinvT11(u1, u2)
	+ dF22(varu1, varu2) * FinvT12(u1, u2)
)//

macro d2Aux21 (u1, u2, varu1, varu2) (
	  dF11(varu1, varu2) * FinvT21(u1, u2)
	+ dF21(varu1, varu2) * FinvT22(u1, u2)
)//

macro d2Aux22 (u1, u2, varu1, varu2) (
	  dF12(varu1, varu2) * FinvT21(u1, u2)
	+ dF22(varu1, varu2) * FinvT22(u1, u2)
)//

///The tensor quantity D3 = F^{-2}_{n} dF_{n+1}
macro d3Aux11 (u1, u2, varu1, varu2, w1, w2) (
	  dF11(varu1, varu2) * FFinv11(u1, u2) * grad11(w1, w2)
	+ dF21(varu1, varu2) * FFinv12(u1, u2) * grad11(w1, w2)
	+ dF11(varu1, varu2) * FFinv21(u1, u2) * grad12(w1, w2)
	+ dF21(varu1, varu2) * FFinv22(u1, u2) * grad12(w1, w2)
)//

macro d3Aux12 (u1, u2, varu1, varu2, w1, w2) (
	  dF12(varu1, varu2) * FFinv11(u1, u2) * grad11(w1, w2)
	+ dF22(varu1, varu2) * FFinv12(u1, u2) * grad11(w1, w2)
	+ dF12(varu1, varu2) * FFinv21(u1, u2) * grad12(w1, w2)
	+ dF22(varu1, varu2) * FFinv22(u1, u2) * grad12(w1, w2)
)//

macro d3Aux21 (u1, u2, varu1, varu2, w1, w2) (
	  dF11(varu1, varu2) * FFinv11(u1, u2) * grad21(w1, w2)
	+ dF21(varu1, varu2) * FFinv12(u1, u2) * grad21(w1, w2)
	+ dF11(varu1, varu2) * FFinv21(u1, u2) * grad22(w1, w2)
	+ dF21(varu1, varu2) * FFinv22(u1, u2) * grad22(w1, w2)
)//

macro d3Aux22 (u1, u2, varu1, varu2, w1, w2) (
	  dF12(varu1, varu2) * FFinv11(u1, u2) * grad21(w1, w2)
	+ dF22(varu1, varu2) * FFinv12(u1, u2) * grad21(w1, w2)
	+ dF12(varu1, varu2) * FFinv21(u1, u2) * grad22(w1, w2)
	+ dF22(varu1, varu2) * FFinv22(u1, u2) * grad22(w1, w2)
)//

///The tensor quantity D4 = (grad w) * Finv
macro d4Aux11 (w1, w2, u1, u2) (
	  Finv11(u1, u2)*grad11(w1, w2)
	+ Finv21(u1, u2)*grad12(w1, w2)
)//

macro d4Aux12 (w1, w2, u1, u2) (
	  Finv12(u1, u2)*grad11(w1, w2)
	+ Finv22(u1, u2)*grad12(w1, w2)
)//

macro d4Aux21 (w1, w2, u1, u2) (
	  Finv11(u1, u2)*grad21(w1, w2)
	+ Finv21(u1, u2)*grad22(w1, w2)
)//

macro d4Aux22 (w1, w2, u1, u2) (
	  Finv12(u1, u2)*grad21(w1, w2)
	+ Finv22(u1, u2)*grad22(w1, w2)
)//
//The macros for the auxiliary tensors (D0, D1, D2, ...): End

//The macros for the various stress measures: BEGIN
//The Kirchhoff stress tensor
macro StressK11(u1, u2) (
	  mu * (B11(u1, u2) - 1.0)
)//

//The Kirchhoff stress tensor
macro StressK12(u1, u2) (
	  mu * B12(u1, u2)
)//

//The Kirchhoff stress tensor
macro StressK21(u1, u2) (
	  mu * B21(u1, u2)
)//

//The Kirchhoff stress tensor
macro StressK22(u1, u2) (
	  mu * (B22(u1, u2) - 1.0)
)//

//The tangent Kirchhoff stress tensor
macro TanK11(u1, u2, varu1, varu2) (
	  mu * d1Aux11(u1, u2, varu1, varu2)
)//

macro TanK12(u1, u2, varu1, varu2) (
	  mu * d1Aux12(u1, u2, varu1, varu2)
)//

macro TanK21(u1, u2, varu1, varu2) (
	  mu * d1Aux21(u1, u2, varu1, varu2)
)//

macro TanK22(u1, u2, varu1, varu2) (
	  mu * d1Aux22(u1, u2, varu1, varu2)
)//
//The macros for the stress tensor components: END

// Parameters
real mu = 5.e2; //Elastic coefficients (kg/cm^2)
real D = 1.e3; //(1 / compressibility)
real Pa = -3.e2; //Stress loads

real InnerRadius = 1.e0; //The wound radius
real OuterRadius = 4.e0; //The outer (truncated) radius
real tol = 1.e-4; //Tolerance (L^2)
real InnerEllipseExtension = 1.e0; //Extension of the inner ellipse ((major axis) - (minor axis))

int m = 40, n = 20;

// Mesh
border InnerEdge(t=0, 2.*pi){x=(1.0 + InnerEllipseExtension)*InnerRadius*cos(t); y=InnerRadius*sin(t); label=1;}
border OuterEdge(t=0, 2.*pi){x=(1.0 + 0.0*InnerEllipseExtension)*OuterRadius*cos(t); y=OuterRadius*sin(t); label=2;}
mesh Th = buildmesh(InnerEdge(-m) + OuterEdge(n));
int bottom = 1, right = 2, upper = 3, left = 4;
plot(Th);

// Fespace
fespace Wh(Th, P1dc);
fespace Vh(Th, [P1, P1]);
Vh [w1, w2], [u1n,u2n], [varu1, varu2];
Vh [ehat1x, ehat1y], [ehat2x, ehat2y];
Vh [auxVec1, auxVec2]; //The individual elements of the total 1st Piola-Kirchoff stress
Vh [ef1, ef2];

fespace Sh(Th, P1);
Sh p, ppp;
Sh StrK11, StrK12, StrK21, StrK22;
Sh u1, u2;

// Problem
varf vfMass1D(p, q) = int2d(Th)(p*q);
matrix Mass1D = vfMass1D(Sh, Sh, solver=CG);

p[] = 1;
ppp[] = Mass1D * p[];

real DomainMass = ppp[].sum;
cout << "DomainMass = " << DomainMass << endl;

varf vmass ([u1, u2], [v1, v2], solver=CG)
	= int2d(Th)( (u1*v1 + u2*v2) / DomainMass );

matrix Mass = vmass(Vh, Vh);

matrix Id = vmass(Vh, Vh);

//Define the standard Euclidean basis functions
[ehat1x, ehat1y] = [1.0, 0.0];
[ehat2x, ehat2y] = [0.0, 1.0];

real ContParam, dContParam;

problem neoHookeanInc ([varu1, varu2], [w1, w2], solver=LU)
	= int2d(Th, qforder=1)(
		- (
			  StressK11 (u1n, u2n) * d3Aux11(u1n, u2n, varu1, varu2, w1, w2)
			+ StressK12 (u1n, u2n) * d3Aux12(u1n, u2n, varu1, varu2, w1, w2)
			+ StressK21 (u1n, u2n) * d3Aux21(u1n, u2n, varu1, varu2, w1, w2)
			+ StressK22 (u1n, u2n) * d3Aux22(u1n, u2n, varu1, varu2, w1, w2)
		)
		+ TanK11 (u1n, u2n, varu1, varu2) * d4Aux11(w1, w2, u1n, u2n)
		+ TanK12 (u1n, u2n, varu1, varu2) * d4Aux12(w1, w2, u1n, u2n)
		+ TanK21 (u1n, u2n, varu1, varu2) * d4Aux21(w1, w2, u1n, u2n)
		+ TanK22 (u1n, u2n, varu1, varu2) * d4Aux22(w1, w2, u1n, u2n)
	)
	+ int2d(Th, qforder=1)(
		  StressK11 (u1n, u2n) * d4Aux11(w1, w2, u1n, u2n)
		+ StressK12 (u1n, u2n) * d4Aux12(w1, w2, u1n, u2n)
		+ StressK21 (u1n, u2n) * d4Aux21(w1, w2, u1n, u2n)
		+ StressK22 (u1n, u2n) * d4Aux22(w1, w2, u1n, u2n)
	)
	//Choose one of the following two boundary conditions involving Pa:
	// Load vectors normal to the boundary:
	- int1d(Th, 1)(
		  Pa * (w1*N.x + w2*N.y)
	)
	//Load vectors tangential to the boundary:
	//- int1d(Th, 1)(
	//	  Pa * (w1*N.y - w2*N.x)
	//)
	+ on(2, varu1=0, varu2=0)
	;

//Auxiliary variables
matrix auxMat;

// Newton's method
ContParam = 0.;
dContParam = 0.01;

//Initialization:
[varu1, varu2] = [0., 0.];
[u1n, u2n] = [0., 0.];
real res = 2. * tol;
real eforceres;
int loopcount = 0;
int loopmax = 45;

// Iterations
while (loopcount <= loopmax && res >= tol){
	loopcount ++;
	cout << "Loop " << loopcount << endl;

	// Solve
	neoHookeanInc;

	// Update
	u1 = varu1;
	u2 = varu2;

	// Residual
	w1[] = Mass*varu1[];
	res = sqrt(w1[]' * varu1[]); //L^2 norm of [varu1, varu2]
	cout << " L^2 residual = " << res << endl;

	// Newton
	u1n[] += varu1[];

	// Plot
	plot([u1n,u2n], cmm="displacement");
}

// Plot
plot(Th, wait=true);

// Movemesh
mesh Th1 = movemesh(Th, [x+u1n, y+u2n]);

// Plot
plot(Th1, wait=true);
plot([u1n,u2n]);
```


## References

<a name="OGDEN1984">[OGDEN1984]</a> OGDEN, Ray W. Non-linear elastic deformations. 1984.

<a name="RAVIART1998">[RAVIART1998]</a> RAVIART, Pierre-Arnaud, THOMAS, Jean-Marie, CIARLET, Philippe G., et al. Introduction à l'analyse numérique des équations aux dérivées partielles. Paris : Dunod, 1998.

<a name="HORGAN2004">[HORGAN2004]</a> HORGAN, Cornelius O. et SACCOMANDI, Giuseppe. Constitutive models for compressible nonlinearly elastic materials with limiting chain extensibility. Journal of Elasticity, 2004, vol. 77, no 2, p. 123-138.
