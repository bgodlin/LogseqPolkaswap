public:: true

- Swap subscribtions
	- PoolXYK reserves subscriptions (`poolXYK.reserves`)
	  id:: 8672cc4d-5ebf-4479-acc4-aa9bc6289ed5
	  collapsed:: true
		- IF
			- Condition
				- AND
					- {{ asset selected within "FROM" input }} !=
						- OR
							- "XOR"
							- "XSTUSD"
					- {{ asset selected within "TO" input }} !=
						- OR
							- "XOR"
							- "XSTUSD"
			- Execution
			  collapsed:: true
				- The requests are done with the following parameters indicated:
					- AND
						- AND
							- *baseAsset* = `XOR`
							- *targetAsset* = {{ asset selected within "FROM" input }}
						- AND
							- *baseAsset* = `XOR`
							- *targetAsset* = {{ asset selected within "TO" input }}
						- AND
							- *baseAsset* = `XSTUSD`
							- *targetAsset* = {{ asset selected within "FROM" input }}
						- AND
							- *baseAsset* = `XSTUSD`
							- *targetAsset* = {{ asset selected within "TO" input }}
		- ELIF
			- Condition
				- AND
					- {{ asset selected within "FROM" input }} =
						- OR
							- `XOR`
							- `XSTUSD`
					- {{ asset selected within "TO" input }} !=
						- OR
							- `XOR`
							- `XSTUSD`
			- Execution
			  collapsed:: true
				- IF
					- Condition
						- {{ asset selected within "FROM" input }} = `XOR`
					- Execution
						- The requests are done with the following parameters indicated:
							- AND
								- AND
									- *baseAsset* = `XOR`
									- *targetAsset* = {{ asset selected within "TO" input }}
								- AND
									- *baseAsset* = `XSTUSD`
									- *targetAsset* = {{ asset selected within "TO" input }}
								- AND
									- *baseAsset* = `XSTUSD`
									- *targetAsset* = `XOR`
				- ELIF
					- Condition
						- {{ asset selected within "FROM" input }} = `XSTUSD`
					- Execution
						- The requests are done with the following parameters indicated:
							- AND
								- AND
									- *baseAsset* = `XSTUSD`
									- *targetAsset* = {{ asset selected within "TO" input }}
								- AND
									- *baseAsset* = `XOR`
									- *targetAsset* = {{ asset selected within "TO" input }}
								- AND
									- *baseAsset* = `XOR`
									- *targetAsset* = `XSTUSD`
		- ELIF
			- Condition
				- AND
					- {{ asset selected within "FROM" input }} !=
						- OR
							- `XOR`
							- `XSTUSD`
					- {{ asset selected within "TO" input }} =
						- OR
							- `XOR`
							- `XSTUSD`
			- Execution
				- IF
					- Condition
						- {{ asset selected within "TO" input }} = `XOR`
					- Execution
						- The requests are done with the following parameters indicated:
							- AND
								- AND
									- *baseAsset* = `XOR`
									- *targetAsset* = {{ asset selected within "FROM" input }}
								- AND
									- *baseAsset* = `XSTUSD`
									- *targetAsset* = `XOR`
								- AND
									- *baseAsset* = `XSTUSD`
									- *targetAsset* = {{ asset selected within "FROM" input }}
				- ELIF
					- Condition
						- {{ asset selected within "TO" input }} = `XSTUSD`
					- Execution
						- The requests are done with the following parameters indicated:
							- AND
								- AND
									- *baseAsset* = `XSTUSD`
									- *targetAsset* = {{ asset selected within "FROM" input }}
								- AND
									- *baseAsset* = `XOR`
									- *targetAsset* = `XSTUSD`
								- AND
									- *baseAsset* = `XOR`
									- *targetAsset* = {{ asset selected within "FROM" input }}
		- ELIF
			- Condition
				- AND
					- {{ asset selected within "FROM" input }} =
						- OR
							- `XOR`
							- `XSTUSD`
					- {{ asset selected within "TO" input }} =
						- OR
							- `XOR`
							- `XSTUSD`
			- Execution
				- The requests are done with the following parameters indicated:
					- AND
						- AND
							- *baseAsset* = {{ asset selected within "TO" input }}
							- *targetAsset* = {{ asset selected within "FROM" input }}
						- AND
							- *baseAsset* = {{ asset selected within "FROM" input }}
							- *targetAsset* = {{ asset selected within "TO" input }}
	- TBC reserves subscription (`multicollateralBondingCurvePool.collateralReserves`)
	  id:: f636dc16-3579-41ee-aa73-be104aa5260e
	  collapsed:: true
		- {{ asset selected within "FROM" input }} is passed as the parameter
- Determination of the swapping values
  id:: 99eade8e-29bc-4c89-9f80-ddf34b4cbf7a
	- SUBSCRIBE
		- Subscriptions
			- PARALLEL Parallelism is introduced in order to maximize the support of the user in the up-to-date state of the market
				- THREAD
					- Handling changes in pool reserves and TBC
						- CASE SMART selected as a market algorithm
							- ((8672cc4d-5ebf-4479-acc4-aa9bc6289ed5))
							- ((f636dc16-3579-41ee-aa73-be104aa5260e))
						- CASE XYK selected as a market algorithm
							- ((8672cc4d-5ebf-4479-acc4-aa9bc6289ed5))
						- CASE TBC selected as a market algorithm
							- ((f636dc16-3579-41ee-aa73-be104aa5260e))
				- THREAD
					- Handling the changes in the assets??? amounts inputs
		- Execution
			- Swap amounts recalculation
				- Quote the conversion amounts
				  id:: eccdfc03-12cc-4f27-b246-3fa96592e84c
					- `liquidityProxy.quote` is being sent indicating all relevant `dexId`
						- AND
							- `dexId` = 0
							- `dexId` = 1
			- More profitable exchange at the current time
				- The one query that is having a larger `amount` received within ((eccdfc03-12cc-4f27-b246-3fa96592e84c)) is considered more profitable at the current moment of time and will be used in the extrinsic
- REPEAT ((99eade8e-29bc-4c89-9f80-ddf34b4cbf7a)) UNTIL Checking the sufficiency of the balance is successful