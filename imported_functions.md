



# Imported Functions



:::{dropdown} Click to see `create_payoff_df`

```py
def create_payoff_df(df, settlement,OLS=False):
    adjusted_maturities = adjust_bond_pay_dates(list(df.index))
    all_maturities = set(adjusted_maturities)


    df_payoff_columns = sorted(all_maturities)
    df_payoff_index=[i for i in range(len(df.index))]


    df_payoff = pd.DataFrame(
        np.zeros((len(df), len(df_payoff_columns))),
        columns=df_payoff_columns,
        index=df_payoff_index
    )
    total_rows = len(df)
    # Define a clean, pleasing HTML template for our status box
    def status_box(current, total):
        return f"""
        <div style="font-family: Arial, sans-serif; padding: 10px 15px; background-color: #f8f9fa; 
                    border-left: 4px solid #007bff; border-radius: 4px; width: fit-content; color: #333;">
            <b style="color: #007bff;">⚙️ Processing Bonds:</b> {current} of {total} added to DataFrame
        </div>
        """
    
    # initial display showing 0 bonds added
    progress_ui = display(HTML(status_box(0, total_rows)), display_id=True)


    for index,(maturity, coupon) in enumerate(zip(df.index, df['Coupon'])):


        # bond_pay_data returns payment dates and amounts
        row_pay_data = bond_pay_data(maturity, coupon, settlement=settlement)


        # Find any dates that aren't already columns
        new_dates = set(row_pay_data[0].flatten()) - all_maturities


        if new_dates:
          if OLS:
            df_clean = df_payoff.loc[(df_payoff != 0).any(axis=1),
                                         (df_payoff != 0).any(axis=0)]
            print("\u2705 DataFrame Complete (Exited Early)!")
            return df_clean
          else:
            # "\u2705 FIX: Add new dates to our master set and reindex
            all_maturities.update(new_dates)
            df_payoff = df_payoff.reindex(columns=sorted(all_maturities), fill_value=0.0)
 
        #    fill up the columns
        df_payoff.loc[index, row_pay_data[0]] = row_pay_data[1]
         # update the progress bar
        progress_ui.update(HTML(status_box(index + 1, total_rows)))
    # re-sort the columns so dates are chronological
    df_payoff = df_payoff.reindex(sorted(df_payoff.columns), axis=1)
    progress_ui.update(HTML("""
        <div style="font-family: Arial, sans-serif; padding: 10px 15px; background-color: #e6f4ea; 
                    border-left: 4px solid #34a853; border-radius: 4px; width: fit-content; color: #137333;">
            <b>"\u2705 DataFrame Complete!</b> All bonds added successfully.
            </div>
            """))
    return df_payoff

```
:::

:::{dropdown} Click to see `ns_spot_rates`

```py
def ns_spot_rates(interim_estimates,mat_years,sofr_rate=None):
  """
    Calculates spot rates using the Nelson-Siegel yield curve model.
    
    Handles both the unrestricted 4-parameter model and a restricted 
    3-parameter model where the short end is pinned to a proxy rate (like SOFR).


    Args:
        interim_estimates (array-like): Current parameter estimates. 
            Expects 3 values (Beta 0, Beta 2, Tau) if sofr_rate is provided.
            Expects 4 values (Beta 0, Beta 1, Beta 2, Tau) if unrestricted.
        mat_years (array-like): Time to maturity for each cash flow, in years.
        sofr_rate (float, optional): The short-term rate to pin the curve to. 
            Defaults to None (triggers unrestricted 4-parameter model).


    Returns:
        tuple:
            - np.ndarray: The calculated spot rates for the given maturities.
            - np.ndarray: The adjusted time-to-maturity array (zeros replaced with 1e-8).
  """
  # t saves typing
  t=mat_years


  # Avoid division by zero for t=0
  t = np.where(t == 0, 1e-8, t)
   
  if sofr_rate is not None:  # SOFR ties download the short rate


    # current values of estimates
    b_0,b_2,tau=interim_estimates
 
    # Restricted Nelson-Siegel model Restricted
    spot_rates = (
            b_0 
            + (sofr_rate - b_0) * (1 - np.exp(-t/tau)) / (t/tau) 
            + b_2 * ((1 - np.exp(-t/tau)) / (t/tau) - np.exp(-t/tau)))
  else:    # SOFR ignored


    # Unrestricted Nelson-Siegel model Unrestricted


    # current values of estimates
    b_0,b_1,b_2,tau=interim_estimates


    spot_rates = (
            b_0 
            + b_1 * (1 - np.exp(-t/tau)) / (t/tau) 
            + b_2 * ((1 - np.exp(-t/tau)) / (t/tau) - np.exp(-t/tau))
        )
  # pass these rates to the objective function for step one
  return spot_rates,t

```
:::

:::{dropdown} Click to see `calc_bond_metrics_2d`

```py
def calc_bond_metrics_2d(guesses, cash_flows, mat_years, prices):
    """
    Calculates YTM, Duration, and Convexity for a single bond OR a portfolio.
    Expects Cash Flows as (Dates x Bonds).
    """
    is_scalar_input = np.isscalar(prices)

    p = np.atleast_1d(prices)
    cf = np.asarray(cash_flows)

    # If a single bond's cash flows are passed as a flat list, stand it up into a column (Dates, 1)
    if cf.ndim == 1:
        cf = cf[:, np.newaxis]

    # Stand the dates up into a column: (Dates, 1)
    t = np.asarray(mat_years)[:, np.newaxis]

    if np.isscalar(guesses):
        guesses = np.full(len(p), guesses, dtype=float)
    else:
        guesses = np.atleast_1d(guesses)

    def ytm_objective(y, cf, t, p):
        # y is (8,). t is (100, 1). y * t automatically broadcasts to (100, 8)
        # Sum down the rows (axis=0) to collapse the dates, leaving (8,) PVs
        pv = np.sum(cf * np.exp(-y * t), axis=0)
        return pv - p

    def ytm_derivative(y, cf, t, p):
        return np.sum(-t * cf * np.exp(-y * t), axis=0)

    # --- STEP 1: Solve for YTM ---
    ytm = optimize.newton(
        func=ytm_objective,
        x0=guesses,
        fprime=ytm_derivative,
        args=(cf, t, p)
    )

    # --- STEP 2: Calculate Risk Metrics ---
    # ytm is (8,). t is (100, 1). discounted_cf becomes (100, 8)
    discounted_cf = cf * np.exp(-ytm * t)

    # Sum down the rows (axis=0) and divide by prices
    duration = np.sum(t * discounted_cf, axis=0) / p
    convexity = np.sum((t**2) * discounted_cf, axis=0) / p

    if is_scalar_input:
        return ytm[0], duration[0], convexity[0]

    return ytm, duration, convexity
```
:::
:::{dropdown} Click to see `calc_bond_metrics_3d`

```py
def calc_bond_metrics_3d(guesses, cash_flows, mat_years, prices):
    """
    Calculates YTM, Duration, and Convexity for a matrix of prices (Scenarios x Bonds).
    Assumes continuous compounding.

    Returns:
        ytm (ndarray): Yield to Maturity
        duration (ndarray): Macaulay/Modified Duration
        convexity (ndarray): Convexity
    """
    cf = np.asarray(cash_flows)              # (100, 8)
    t = np.asarray(mat_years)[:, np.newaxis] # (100, 1)
    p = np.asarray(prices)                   # (6, 8)
    is_1d = False
    if p.ndim == 1:
        is_1d = True
        p = p[np.newaxis, :]  # Temporarily promote to (1, Bonds)
    # Broadcast the initial guess
    if np.isscalar(guesses):
        guesses = np.full_like(p, guesses, dtype=float)
    else:
        guesses = np.broadcast_to(guesses, p.shape)

    def ytm_objective(y, cf, t, p):
        # Insert the Dates dimension in the middle: (6, 1, 8)
        y_3d = y[:, np.newaxis, :]

        # Multiply to get (6, 100, 8), then sum across Dates (axis=1) -> (6, 8)
        pv = np.sum(cf * np.exp(-y_3d * t), axis=1)
        return pv - p

    def ytm_derivative(y, cf, t, p):
        y_3d = y[:, np.newaxis,:]
        return np.sum(-t * cf * np.exp(-y_3d * t), axis=1)

    # --- STEP 1: Solve for YTM ---
    ytm = optimize.newton(
        func=ytm_objective,
        x0=guesses,
        fprime=ytm_derivative,
        args=(cf, t, p)
    )

    # --- STEP 2: Calculate Risk Metrics ---
    # Convert the final YTM grid into 3D: (Scenarios, 8, 1)
    y_3d_final = ytm[:, np.newaxis,:]

    # Calculate the final discounted cash flows matrix: (Scenarios, 8, 100)
    discounted_cf = cf * np.exp(-y_3d_final * t)
    # Calculate Duration and Convexity
    # We divide directly by 'p' (the market prices) because the optimizer
    # just guaranteed that sum(discounted_cf) == p
    duration = np.sum(t * discounted_cf, axis=1) / p
    convexity = np.sum((t**2) * discounted_cf, axis=1) / p

    return ytm, duration, convexity

```
:::

