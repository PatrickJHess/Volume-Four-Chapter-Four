# Financial Python
## Volume: Pricing And Interest Rate Risk
## Chapter Four: 📈 Interest Rate Risk: From Static Metrics to Dynamic Simulation

### **The Moving Target of Duration, Convexity, and Yield to Maturity**

This Chapter introduces interest rate risk by focusing on two traditional measures derived directly from the present value formula:

* **Duration**: The first derivative of the present value formula, scaled by value.

* **Convexity**: The second derivative of the present value formula, scaled by value.

Both measures are purely mathematical, and in that sense, they are very powerful. It is only natural to apply these concepts when analyzing how bond prices change. In calculus, knowing the first and second derivatives of a function is usually sufficient to predict how its output will react to changes in its inputs.

There is, however, a catch in this instance.

From inspecting the present value formula and our knowledge of the term structure, a bond that makes multiple payments has multiple first and second-degree partial derivatives. While we can aggregate those partial derivatives, that sum does not relate to any single spot rate of interest. Little, if anything, is accomplished by that raw calculation.

So why are duration and convexity widely regarded as useful measures of interest rate risk? The answer lies in their close cousin: **Yield to Maturity (YTM)**.

As we saw in the previous Chapter, we can define a single discount rate—the yield to maturity—to price a bond. Once established, the duration and convexity of a bond are computed relative to this YTM. The industry standard is to measure interest rate risk using the first and second derivatives defined with the yield to maturity.

It is a mathematical sleight of hand. As we also demonstrated previously, a bond's yield to maturity is merely a complex aggregation of the underlying spot rates used to discount its cash flows. So what are duration and convexity really? Like yield to maturity, they are useful rules of thumb.

The main takeaway for this Chapter is that duration and convexity are static measures; they provide a general glimpse, but not a detailed prediction, of how bond prices will actually vary with complex changes in the term structure.

#### The Road Ahead

To bridge the gap between textbook theory and trading floor reality, this chapter is built around two interactive notebooks:

1. **🎢 The Dynamics of Interest Rate Risk Stress-Testing Portfolios Across the Yield Curve**

We begin by establishing the baseline mechanics of traditional risk metrics. You will learn how to calculate and apply first and second-order derivatives (duration and convexity) to price changes, setting the mathematical baseline for our risk management framework and observing how portfolios react to standard curve shifts.

2. **🛡️ Beyond Duration & Convexity: Simulating Risk to Drive Strategy Simulating Risk to Drive Strategy**

In the second notebook, we shatter the basic assumptions of static metrics. Deploying Copula-driven Monte Carlo simulations and Nelson-Siegel term structure models, we will stress-test Barbell and Bullet portfolios to reveal where tail risk truly hides. By the end, you will understand exactly why relying on a single risk number is a dangerous game.
