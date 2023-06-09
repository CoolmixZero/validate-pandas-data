# Validate pandas data with pandera

Simple example of how to use Pandera package with Pandas to validate your data.

1. The code defines a `OutputSchema` class that represents a schema for a retail products dataset. This class inherits from `pa.SchemaModel`, which is a class from the `pandera` library for defining data validation and cleaning schemas. The `OutputSchema` class specifies the expected data types and validation constraints for each column of the DataFrame.

2. The `retrieve_retail_products` function is defined with the input parameter `path` which is of type `Path` and returns a pandas DataFrame with the schema defined by the `OutputSchema` class. The `pa.check_types` decorator is used to ensure that the input parameters and the return value of this function conform to the schema defined by `OutputSchema`. If any of the data validation checks fails, a `pa.errors.SchemaErrors` exception is raised.

3. The `main` function calls the `retrieve_retail_products` function with the path of the input data file and prints the first five rows of the resulting DataFrame using the `head()` method. If any of the data validation checks fail, the exception message is printed instead.

___
`online_retail.csv`:

| InvoiceNo | StockCode | Description                            | Quantity | InvoiceDate          | UnitPrice | CustomerID | Country         |
|-----------|-----------|----------------------------------------|----------|----------------------|-----------|------------|----------------|
| 536365    | 85123A    | WHITE HANGING HEART T-LIGHT HOLDER      | 6        | 2010-12-01 08:26:00  | 2.55      | 17850.0    | United Kingdom |
| 536365    | 71053     | WHITE METAL LANTERN                    | 6        | 2010-12-01 08:26:00  | 3.39      | 17850.0    | United Kingdom |
| 536365    | 84406B    | CREAM CUPID HEARTS COAT HANGER          | 8        | 2010-12-01 08:26:00  | 2.75      | 17850.0    | United Kingdom |
| 536365    | 84029G    | KNITTED UNION FLAG HOT WATER BOTTLE     | 6        | 2010-12-01 08:26:00  | 3.39      | 17850.0    | United Kingdom |

---

### Code output:

```python
Schema OutputSchema: A total of 3 schema errors were found.

Error Counts
------------
- SchemaErrorReason.SCHEMA_COMPONENT_CHECK: 3

Schema Error Summary
--------------------
failure_cases  n_failure_cases
schema_context column      check


Column         Country     dtype('string[python]')
                                                                                                                                 [object]
 1
               InvoiceDate dtype('datetime64[ns]')
                                                                                                                                    [object]
 1
               Quantity    greater_than_or_equal_to(1)  [-1, -3, -2, -4, -5, -13, -9, -188, -31, -635, -12, -117, -3667, -40, -6, -11, -21, -7, -14, -140,
-19, -10, -27, -8, -22, -20, -15, -36, -17, -32, -756, -752, -24, -152, -53, -23, -48, -384, -44, -86, -29, -54, -63, -62, -16, -49, -138, -126, -113, -180, -115, -66, -77, -60, -110, -46, -18, -242, -144, -318, -160, -72, -120, -210, -967, -203, -156, -3167, -252, -443, -432, -1897, -480, -109, -334, -250, -200, -400, -52, -95, -682, -484, -100, -33, -434, -38, -2880, -1200, -30, -1440, -70, -275, -458, -69, -130, -129, -25, -28, -380, -45, ...]              329

Usage Tip
---------

Directly inspect all errors by catching the exception:


try:
    schema.validate(dataframe, lazy=True)
except SchemaErrors as err:
    err.failure_cases  # dataframe of schema errors
    err.data  # invalid dataframe
```
