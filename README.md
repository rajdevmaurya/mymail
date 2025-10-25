

We identified an issue in our OpenAPI schema where a field was defined as an enum, but the actual API response contained dynamic string values.
Because of this mismatch, the generated Java code expected an enum constant and failed at runtime with an error like:

Unexpected value ''

Root Cause:
The OpenAPI specification used an enum reference under additionalProperties, which restricted the values to a fixed list.
However, our real data can vary for each user (e.g., "", "", "").

Fix Implemented:
We changed the OpenAPI field definition from enum to string type:

# Before
additionalProperties:
  $ref: '#/components/schemas/IType'

# After
additionalProperties:
  type: string
Outcome:

The generated model now correctly uses Map<String, String>.

No more enum conversion errors.

API response behavior remains unchanged.

The spec now accurately represents the actual runtime data.# mymail
