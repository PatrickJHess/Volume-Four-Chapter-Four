# **📉  Interest Rate Risk-Duration and Convexity**

## **⏱️ What is duration and why does it matter**

Albert Einstein famously called compound interest the eighth wonder of the world, noting that those who understand it earn it, and those who don't pay it.

A less poetic but equally vital truth is that the present value of any future cash flow is entirely hostage to two things: time and interest rates. When interest rates climb, the math of compounding aggressively erodes the current value of future payments. When rates fall, the reverse happens, lifting their value today.

This constant tug-of-war is the root of interest rate risk—the inherent volatility that bond investors must navigate. Building an effective financial strategy requires mastering this risk, a process that begins with a single, critical metric: duration.

## **🔍 Defining duration**

Previously we noted that yields to maturity are best calculated with the so-called Newton-Raphson method.  That procedure used the first derivative of bond prices with respect to their yields to maturity  to search for a value.  The  equation iteratively solved is:
<br>

$$V(\text{y}+\Delta \text{y})-V(\text{y})\approx -\Delta \text{y}\times\sum_{i=1}^{N}t_i\times \text{Cash Flow}_{t_i} \times e^{-\text{y}\times t_i}$$

Because the first derivative tells where to go when we are searching for the yield to maturity, it also tells how a bond price reacts to changes in the yield to maturity-an obvious measure of interest rate risk.

The rate of change in the value of the bond equals the change in value divided by its initial value:

$$\frac{V(\text{y}+\Delta \text{y})-V(\text{y})}{V(\text{y})}\approx -\Delta \text{y}\times\frac{\sum_{i=1}^{N}t_i\times \text{Cash Flow}_{t_i} \times e^{-\text{y}\times t_i}}{\text{V(y)}}$$

The last term on the right-hand side of the equation is the bond's duration:

$$\text{Duration} = \frac{\sum_{i=1}^{N}t_i\times \text{Cash Flow}_{t_i} \times e^{-\text{y}\times t_i}}{\text{V(y)}}$$

Duration scales the effect of changes in yield to maturity on bond values Duration measures a bond's interest rate risk by showing how sensitive its price is to changes in the yield to maturity. Specifically, a duration of one forecast a percentage price change equal to the change in the yield to maturity. A qualification, however, is needed. Because duration is negatively related to the yield to maturity, forecasts of price changes will contain errors that are also negatively related to changes in yields to maturity-forecasts will be positive for decreases and negative for increases in yields to maturity.

## **⚖️ Duration Is Weighted Average Of A Bond's Payment Dates**

Duration is a weighted average of a bond's payment dates. The weights are determined by dividing the present value of each payment by the bond's total present value. It is calculated by summing the products of each cash flow's present value and its payment date, and then dividing this sum by the bond's present value.

$$\text{Duration} = \sum_{i=1}^{N}t_i\times w_{t_i}$$

$$ w_{t_i} =\frac{\text{Cash Flow}_{t_i}\times e^{-\text{y}\times t_i}}{V(y)}$$

$$\sum_{i=1}^{N}w_{t_i} = 1$$

The duration of a bond that makes a single payment at T is its maturity.🎯

$$\text{T}= \frac{\text{T}\times \text{Principal}_T\times e^{-\text{y}\times \text{T}}}{ \text{Principal}_T\times e^{-\text{y}\times \text{T}}}$$

The timing of bond payments before maturity reduces its duration, making it shorter than the maturity date. For bonds with multiple coupon payments, a higher coupon rate leads to a lower duration.💵

## **📅 Measuring duration with discrete compounding**

Our discussion and calculations assume continuous compounding. With continuous compounding, duration is simply equal to the weighted average maturity. The weights used in this calculation are the present value of each payment divided by the present value of all payments.

For discrete compounding, two primary duration measures are used: Macaulay's Duration and Modified Duration.  Macaulay's Duration is a measure of the weighted average term to maturity of a bond's cash flows, defined by the formula:

$$\text{Macaulay's Duration} = \frac{\sum_{i=1}^{N}t_i \times \text{Cash Flow}_{t_i} \times (1+y)^{-t_i}}{\text{V(y)}}$$

This measure is closely related to the first derivative of the present value formula with discrete compounding:

$$\text{First Derivative} = \frac{\sum_{i=1}^{N}t_i \times \text{Cash Flow}_{t_i} \times (1+y)^{-t_i}}{(1+\text{y})}$$

Modified Duration adjusts Macaulay's Duration by dividing it by one plus the yield (1+y). This measure forecasts the relative change in present value for a given change in yield, similar to the continuous compounding definition of duration.

$$\text{Modified Duration} = \frac{1}{1+\text{y}} \times \frac{\sum_{i=1}^{N}t_i \times \text{Cash Flow}_{t_i} \times (1+y)^{-t_i}}{\text{V(y)}}$$

## **🤔 What Interest Rates Risk Does Duration Measure**

Using duration to gauge a bond's interest rate risk—specifically, its sensitivity to interest rate increases—leads to a key question: which interest rates? Because a coupon bond is essentially a portfolio of zero-coupon bonds, its yield to maturity is a complex blend of various spot rates. The yield depends on the bond's maturity, its coupon, and the entire sequence of spot rates.

A change in short-term spot rates (either up or down) can be entirely offset by an opposite change in long-term spot rates. This means the term structure of interest rates has three components: a level (the overall height), a direction (upward or downward shift), and a shape (its curvature). **Because traditional duration assumes a parallel shift across a flat yield curve, it acts as a convenient rule of thumb rather than a perfect predictor.** When the yield curve twists, steepens, or inverts, this rule of thumb breaks down, meaning caution is called for when relying on it in dynamic environments. This conundrum is at the heart of understanding the limitations and the uses of duration.

 It's natural to draw parallels between duration and the Capital Asset Pricing Model's (CAPM) beta, it's important to recognize a key difference: beta is a parameter of an equilibrium model, offering insights beyond mere empirical observation. Duration is less profound; it is a powerful mathematical tool but does not offer predictions about expected bond returns.

## **🎢 What is convexity and why does it matter**

If the present value formula is the grandfather, duration is the son, and convexity is the grandson. It is simply the second derivative of the present value formula with respect to the yield to maturity, but, of course, that is the first derivative of duration.  Our generational characterization of duration and convexity is more than a mathematical result.  It profoundly affects how duration should be used to manage a fixed income portfolio

### **🔢 Introducing Convexity via Taylor Series Expansion**

The general form of a Taylor series expansion is:

$$f(x+\Delta x)\approx\sum_{i=1}^{N}\frac{d^{i}f(x)}{dx^{i}}\times\frac{1}{i\!}\Delta x^{i}$$

Convexity is defined using the second-order Taylor series expansion:

$$f(x+\Delta x)\thickapprox f(x)+\frac{df(x)}{dx}\times\Delta x+\frac{1}{2}\frac{d^{2}f(x)}{dx^{2}}\times\Delta x^{2}$$

Applying this second-order expansion to the present value formula yields a more accurate estimate of the change in bond values.🎯

### **🧮 The Convexity Formula**

Mirroring the definition of duration, convexity is calculated by dividing the bond's second derivative with respect to $\text{y}$ by the bond's value:

$$Convexity =\frac{\sum_{i=1}^{N}t_i^{2}\times Cash\ Flow_{t_i} \times e^{-\text{y}\times t_i}}{V(\text{y})}$$

## **🔮 Convexity and Duration Predict Bond Price Changes**

The change in bond price ($\Delta V$) can be predicted using a second-order Taylor expansion that incorporates both duration and convexity, as shown below:

$$\Delta V \approx (Duration \times -\Delta \text{y} + \frac{1}{2} \times Convexity \times \Delta \text{y}^{2}) \times V(\text{y})$$ 

The change in yield to maturity ($\Delta \text{y}$) can be broken down into its expected value ($E(\Delta \text{y})$) and its deviation from that expected value:

$$\Delta \text{y} = (\Delta \text{y} - E(\Delta \text{y}) + E(\Delta \text{y})$$

The squared change in yield to maturity is:

$$\Delta \text{y}^{2} = (\Delta \text{y} - E(\Delta \text{y})^{2} + E(\Delta \text{y})^{2}+2 \times (\Delta \text{y} - E(\Delta \text{y}) \times E(\Delta \text{y})$$

Consequently, the expected value of the squared change in yield to maturity is:

$$E(\Delta \text{y}^{2}) = \sigma_{\Delta \text{y}}^{2}+E(\Delta \text{y})^{2}$$

Substituting the expected values into the Taylor expansion, the expected change in the bond's value is:

$$E(\Delta V) =(Duration \times -E(\Delta \text{y})+ \frac{1}{2} \times Convexity \times (\sigma_{\Delta \text{y}}^{2}+E(\Delta \text{y})^{2}) \times V(\text{y})$$

The second-order Taylor expansion reveals two primary effects on the expected bond price changes:

* $\qquad🧭 \textbf{Direction:}\quad   -Duration\times E(\Delta \text{y})$  
* $\qquad⚡ \textbf{Volatility:}\quad  \frac{1}{2}\times Convexity\times (\sigma^{2}_{\Delta \text{y}}+E(\Delta \text{y})^2)$

## **💼 Convexity and Duration: A Portfolio Perspective**

The duration of a bond is calculated as the weighted average of the time to each of the bond's payments. The weight for each payment is determined by the present value of that cash flow relative to the total value of the bond.

$$\text{Duration} = \sum_{i=1}^{N}t_i \times w_{t_i}$$

$$\text{Where } w_{t_i} = \frac{\text{Cash Flow}{t_i} \times e^{-\text{y} \times t_i}}{V(-\text{y})} \quad \text{and} \quad \sum{i=1}^{N}w_{t_i} = 1$$

Similarly, the bond's convexity is defined as the weighted average of the squared payment dates, using the identical weights as those used for duration:

$$\text{Convexity} = \sum_{i=1}^{N}t_i^{2} \times w_{t_i}$$

The concepts of duration and convexity fundamentally incorporate the arbitrage principle, much like the present value calculation. A bond can be conceptualized as a composite portfolio of zero-coupon bonds, where each component bond represents a single cash flow weighted by its present value. Consequently, a bond's overall duration and convexity are simply the sum of the durations and convexities of these individual cash flows, weighted by their contribution to the total portfolio value. This analogy highlights that duration and convexity of a bond is akin to the convexity of a portfolio and that is simply the weighted average of the duration and convexity of all the cash flows of the portfolio.

## **🔭 Beyond Rules of Thumb: The Dynamic Term Structure**

While Macaulay duration, Modified duration, and convexity provide excellent mathematical baselines, they are ultimately static snapshots based on simplified assumptions. In reality, interest rate risk is deeply dynamic, driven by a constantly evolving term structure.

A portfolio's risk profile can shift dramatically depending on how cash flows are distributed across that curve. For instance, two portfolios with the exact same duration—such as a heavily concentrated "Bullet" portfolio and a widely dispersed "Barbell" portfolio—will react entirely differently to non-parallel shifts in the yield curve.

Because real-world yield curves rarely move in perfect, parallel lockstep, advanced financial strategies require moving beyond static rules of thumb. To accurately measure and manage interest rate risk, we must simulate how these portfolios perform under dynamic term structures. By utilizing models like the Nelson-Siegel framework, we can capture the actual level, slope, and curvature of interest rates, revealing the true risk and return dynamics of these fixed-income strategies.

### **📈 Modeling the Curve: The Nelson-Siegel Framework**

To understand how differently structured portfolios—like our Barbell and Bullet strategies—truly behave, we must quantify the yield curve's movements. Rather than shifting every spot rate individually, we can parameterize the entire term structure using the **Nelson-Siegel model**.

This framework reduces the infinite complexity of the yield curve into a smooth, continuous function driven by four distinct parameters:

$$r(t) = \beta_0 + \beta_1 \left( \frac{1 - e^{-t/\tau}}{t/\tau} \right) + \beta_2 \left( \frac{1 - e^{-t/\tau}}{t/\tau} - e^{-t/\tau} \right)$$
Where:

* **📏$\beta_0$ (Level):** The long-term interest rate. A change here shifts the entire curve up or down.  
* **📐$\beta_1$ (Slope):** The short-term component. It dictates the steepness or inversion of the curve.  
* **🐪$\beta_2$ (Curvature):** The medium-term component. It creates the "hump" or "dip" in the middle of the curve.  
* **⏱️$\tau$ (Scale):** A decay factor that determines where the curvature ($\beta_2$) achieves its maximum impact.

By anchoring these parameters to real-world rates (such as SOFR), we can generate highly realistic, non-parallel curve shifts.

### **🛣️ The Road Ahead: From Stylized Regimes to Copula Simulations**
#### **Comparing the Barbell and Bullet Stragies (🏋️ and 🎯)**
In the **first notebook**, we will stress-test our Barbell and Bullet portfolios across six stylized Nelson-Siegel term structure regimes. We will demonstrate how duration and convexity vary dynamically as the curve levels, steepens, and humps. This exploration culminates in a visual mapping of the portfolios' diverging price behaviors under extreme curve stress, proving why static duration is an incomplete metric.

However, in the real world, the level, slope, and curvature of interest rates do not move in isolated, stylized vacuums; they move together, with complex dependencies.

Therefore, in the **second notebook**, we transition from stylized scenarios to rigorous empirical modeling. We will model the joint distribution of $\beta_0$, $\beta_1$, and $\beta_2$ using **copulas**. By capturing the historical correlations and tail dependencies of these parameters, we will run Monte Carlo simulations to generate thousands of realistic yield curve evolutions, allowing us to observe the true risk, return, and convexity advantages of our portfolios in the wild.
