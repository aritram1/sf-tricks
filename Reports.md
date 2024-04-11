It is possible to pass values to a Salesforce report's filters by `pcX`, `pvX` and `pnX` parameters (X represents 1, 2, 3 etc as per report's existing filters) in URL like below (broken due to brevitiy)

/00OU0000000aYlZ
?`pc0`=ACCOUNT_ID
&`pn0`=eq
&`pv0`={!Account.Id}
&`pc1`=WON
&`pn1`=eq
&`pv1`=TRUE

```
There are three fields in the criteria that you will need to pass data to in order to fully control the criteria / filters in the existing reports

  - `pc0`: Corresponds to the field in the report criteria.
  - `pn0`: Corresponds to the operator in the report criteria.
  - `pv0`: Corresponds to the value in the report criteria.

These inputs are , like an array, 0 based, meaning that you will access the first set of filter attributes (name, operator and value) using 0 base dIndex (e.g., `pc0`, `pn0`, `pv0`).
As you add more filters, you just increase the number (`pc1`, `pn1`, `pv2`, etc.).

Now to the 2 letter operators (pnX params). Here are the values that Salesforce has to offer:

- `eq` = equals
- `ne` = not equals
- `lt` = less than
- `le` = less than or equals
- `gt` = greater than
- `ge` = greater than or equals
- `co` = contains
- `nc` = does not contain
- `sw` = starts with
- `in` = includes

Assume we have an existing report on Opportunity with Report Id as '00OU0000000aYlZ', with few existing filters like
- parent account's id = <some value> and
- Opportunity won = true,
then the following would be the way to construct the URL params in a way, we can pass the values to the filters.

Bringing it all together,
- run the desired report and copy the report ID from the URL.
- We'll use our Won Opportunities report for this example (<https://<someinstance>.salesforce.com/00OU0000000aYlZ>). Paste the report ID into the link window.
- You can then append filter parameters at your leisure (following above rules, a sample params list will look lik ebelow)

"/00OU0000000aYlZ?pc0=ACCOUNT_ID&pn0=eq&pv0={!Account.Id}&pc1=WON&pn1=eq&pv1=TRUE"

In the example above,
- we are passing the Account ID dynamically based on the record the user is in when he/she clicks on the link.

Keep in mind that you can use the methods we have covered here to override filter criteria for existing reports, allowing you to reuse them when necessary instead of having to create a
report with predefined filters where you only pass the filter value (`pv`). There are things you will need to tweak here and there. For example, when passing currencies in your `pv` parameter,
you will want to `URLENCODE()` to make sure that value gets passed correctly.

Source: [Salesforce Developer Forums](https://developer.salesforce.com/forums/?id=906F00000008q0sIAA)
